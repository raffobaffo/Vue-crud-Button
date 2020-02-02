<template>

    <div :style="containerStyle">
        <transition
                v-bind:css="false"
                v-on:enter="enter"
                v-on:leave="leave"
        >
            <div ref="buttonRef"

                 v-show="showButton"

                 class="rbf-main"

                 @mouseover="hover = true; "

                 @mouseleave="hover = false "

                 @click="emitClicking"

                 :style="buttonStyling">

                <a ref="textRef">
                    {{activeText}}
                </a>
            </div>
        </transition>
    </div>

</template>

<script>

    /*
       Default style defined as constant. Probaly will be moved in an external file in next versio
       in order to allow user to set is own default and leave the markup as clean as possible
     */
    const stylingDefaults = {
        active: {

            'backgroundColor': '#fff',
            'backgroundColorHover': '#4b4b4b',
            'borderColor': '#575757',
            'borderColorHover': '#2e2e2e',
            'color': '#4b4b4b',
            'colorHover': '#fcfcfc',
            'fontSize': 'larger',
            'fontWeight': 800
        },

        acting: {

            'backgroundColor': '#fff',
            'backgroundColorHover': '#ff8900',
            'borderColor': '#ff8900',
            'borderColorHover': '#ff6405',
            'color': '#ff6405',
            'colorHover': '#fff',
            'fontSize': 'larger',
            'fontWeight': 800
        },

        inactive: {

            'backgroundColor': '#fff',
            'backgroundColorHover': '#f2f2f2',
            'borderColor': '#e1e1e1',
            'borderColorHover': '#bfbfbf',
            'color': '#848484',
            'colorHover': '#484848',
            'fontSize': 'larger',
            'fontWeight': 800
        }
    };

    import Velocity from 'velocity-animate'

    export default {
        name: "Button.vue",

        props: {

            acting: {
                type: Boolean,
                default: false
            },

            active: {
                type: Boolean,
                required: true,
                default: true
            },

            keepPosition: {
                type: Boolean,
                required: false,
                default: false
            },

            positions: {

                type: Object,
                default: function () {
                    return { x: 'center',
                        y: 'middle'
                    }
                }


            },

            inactiveMessage: {
                type: String,
                required: false,
                default: 'Clicked',

            },
            actingMessage: {
                type: String,
                required: false,
                default: 'Wait'

            },

            activeMessage: {
                type: String,
                required: false,
                default: 'Click me'

            },


            buttonStyle: {

                type: Object,
                default: function () {
                    return {
                        stylingDefaults
                    }
                }


            },


            animate: {

                type: Boolean,
                required: false,
                default: true
            }



        },

        data() {
            return {

                position: {},
                hover : false,
                showButton: true,
                animationComplete : true,
                animationDuration: 200,
                mixedStyling : stylingDefaults

            }
        },

        mounted(){
            this.position = this.$refs.buttonRef.getBoundingClientRect()
            this.mixPropAndDefaultProps();

        },

        watch: {

            acting: function(){
                this.animationDuration = 700;
                if(this.animationComplete){
                    this.showButton = false;
                } else {
                    this.stopAnimationAndContinue();
                }
            },

            active: function(){

                this.animationDuration = 700;
                if(this.animationComplete){
                    this.showButton = false;

                } else {
                    this.stopAnimationAndContinue();
                }
            },

            hover: function () {
                this.animationDuration = 200;

                if (this.animationComplete) {
                    this.showButton = false;
                } else {

                    this.stopAnimationAndContinue();
                }
            }
        },

        computed: {

            status() {

                if(this.acting) { return 'acting' }
                if(!this.active ) {return 'inactive' }

                return 'active'

            },

            containerPositioning(){

                let x, y;

                switch (this.positions.x){

                    case 'left':
                        x = 'margin-right:auto;';
                        break;

                    case 'right':
                        x = 'margin-left:auto;';
                        break;

                    default:
                        x = 'margin-left:auto;margin-right:auto;';
                        break;
                }


                switch (this.positions.y){

                    case 'top':
                        y = 'margin-bottom:auto;'
                        break;

                    case 'bottom':
                        y = 'margin-top:auto;'
                        break;

                    default:
                        y = 'margin-top:auto;margin-bottom:auto;';
                        break;

                }

                return x+y;

            },

            containerStyle(){

                let style  = 'position:relative;'+this.containerPositioning;

                return style;
            },

            activeText() {

                switch (this.status){
                    
                    case 'acting':
                        return this.actingMessage;
                        
                    case 'inactive':
                        return this.inactiveMessage;

                    default:
                        return this.activeMessage;
                }

            },

            staticStyle(){

                switch (this.status){

                    case 'acting':
                        if(!this.hover) {
                            return 'border-color:' + this.mixedStyling.acting.borderColor+';color:'+this.mixedStyling.acting.color+';background-color:'+this.mixedStyling.acting.backgroundColor+';';
                        }
                        return 'border-color:' + this.mixedStyling.acting.borderColorHover+';color:'+this.mixedStyling.acting.colorHover+';background-color:'+this.mixedStyling.acting.backgroundColorHover+';';
                    case 'inactive':
                        if(!this.hover) {
                            return 'border-color:' + this.mixedStyling.inactive.borderColor+';color:'+this.mixedStyling.inactive.color+';background-color:'+this.mixedStyling.inactive.backgroundColor+';';
                        }
                        return 'border-color:' + this.mixedStyling.inactive.borderColorHover+';color:'+this.mixedStyling.inactive.colorHover+';background-color:'+this.mixedStyling.inactive.backgroundColorHover+';';

                    default:
                        if(!this.hover) {
                            return 'border-color:' + this.mixedStyling.active.borderColor+';color:'+this.mixedStyling.active.color+';background-color:'+this.mixedStyling.active.backgroundColor+';';
                        }
                        return 'border-color:' + this.mixedStyling.active.borderColorHover+';color:'+this.mixedStyling.active.colorHover+';background-color:'+this.mixedStyling.active.backgroundColorHover+';';

                }

            },

            buttonStyling() {


                let style = 'vertical-align:middle;align-text:center;';

                this.active && !this.acting ? style += 'cursor:pointer;' : null;

                (! this.animate) ? style += this.staticStyle : null;

                return style;
            },

            statusBorder() {

                switch (this.status){

                    case 'acting':
                        return  (! this.hover) ?  this.mixedStyling.acting.borderColor : this.mixedStyling.acting.borderColorHover;

                    case 'inactive':

                        return  (! this.hover) ?  this.mixedStyling.inactive.borderColor : this.mixedStyling.inactive.borderColorHover;

                    default:
                        return  (! this.hover) ?  this.mixedStyling.active.borderColor : this.mixedStyling.active.borderColorHover;
                }

            },

            statusColor() {

                switch (this.status){

                    case 'acting':
                        return  (! this.hover) ?  this.mixedStyling.acting.color : this.mixedStyling.acting.colorHover;

                    case 'inactive':
                        return  (! this.hover) ?  this.mixedStyling.inactive.color : this.mixedStyling.inactive.colorHover;

                    default:
                        return  (! this.hover) ?  this.mixedStyling.active.color : this.mixedStyling.active.colorHover;
                }

            },

            statusBackgroundColor() {

                switch (this.status){

                    case 'acting':

                        return  (! this.hover) ?  this.mixedStyling.acting.backgroundColor : this.mixedStyling.acting.backgroundColorHover;

                    case 'inactive':

                        return  (! this.hover) ?  this.mixedStyling.inactive.backgroundColor : this.mixedStyling.inactive.backgroundColorHover;

                    default:

                        return  (! this.hover) ?  this.mixedStyling.active.backgroundColor : this.mixedStyling.active.backgroundColorHover;
                }

            }



        },

        methods: {

            mixPropAndDefaultProps(){

                let mixed = {}
                for (let styleProp in stylingDefaults){

                    mixed[styleProp] = stylingDefaults[styleProp];
                    if (this.buttonStyle[styleProp]){

                        for(let parentStyled in this.buttonStyle[styleProp]){
                            mixed[styleProp][parentStyled] = this.buttonStyle[styleProp][parentStyled];
                        }

                    }

                }

                this.mixedStyling = mixed

            },


            async stopAnimation(){

                Velocity.animate(this.$refs.buttonRef, "finish")
                    .then(function() { })

                    .catch(function() { });



            },

            stopAnimationAndContinue(){

                let vm = this
                this.stopAnimation().then( () => {

                    setTimeout( () => {
                        vm.animationComplete = true
                        vm.showButton = false;
                    }, 50 )


                });


            },

            emitClicking(){
                this.showButton = false;
                if(this.active) {
                    this.$emit('click')
                }
            },


            enter: function (el, done) {
                if(! this.animate ) { return }

                let vm = this;
                vm.animationComplete = false

                Velocity(el,
                    { opacity: 1 },
                    {
                        queue: false,
                        duration: 1,
                        complete: function () {
                            done()
                            vm.showButton = true
                            vm.animationComplete = true

                        }
                    }
                )
            },
            leave: function (el, done) {
                if(! this.animate ) { return }

                let vm = this
                vm.animationComplete = false

                Velocity(el,
                    {
                        borderColor: vm.statusBorder,
                        color: vm.statusColor,
                        backgroundColor: vm.statusBackgroundColor,
                    },
                    {

                        queue: false,
                        duration: this.animationDuration,
                        complete: function () {
                            done()
                            vm.animationComplete = true
                            vm.showButton = true

                        }
                    }
                )
            },

        }
    }


</script>

<style lang="css" scoped>

    .rbf-main {

        border: solid 0.2em;
        font-size: larger;
        font-weight: 800;
        padding-top: 0.25em;
        padding-bottom: 0.25em;
        padding-left: 1em;
        padding-right: 1em;
        border-radius: 5em;

    }

</style>
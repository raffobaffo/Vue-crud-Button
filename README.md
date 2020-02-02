# CRUD Button

## 📖 Usage


### 1. Install:
```bash
npm i @raffobaffo/vue-wait-button
```

### 2. Require:
```js

    import Button from '@raffobaffo/vue-wait-button'

    export default {
        name: 'app',
        components: {
            Button
        }
```

### 3. Use in Your Components

```vue
<template>
  <div style="display: flex;flex-direction: column;padding: 1em;">
 
         <Button @click="clickFakeSave()"
                 :animate = "true"
                 :active="shouldSave"
                 :acting="isSaving"
                 :inactiveMessage="'No changes to save'"
                 :activeMessage="'Save changes'"
                 :actingMessage="'Saving'" />
       </div>
```

## ✔ `Vue-Crud-Button` Props

Vue-Crud-Button props. name, type, extras:

| Option Name | Type | Default | Description |
| ----------- | ---- | ------- | ----------- |
| `acting` | `Boolean` | `false` |  Should be `True` when `@click` callback can't be called because a CRUD operation is in progress. Ex. Saving changes in description field | 
| `active` | `Boolean` | `true` | Should be `True` when `@click` callback can be called. Ex. Description text changed | 
| `positions` | `Object` | `x: 'center', y: 'middle' ` | X and Y position of the component inside parent container |
| `activeMessage` | `String` | `"Click me"` | Text to display inside the button when  `@click` callback can be called |
| `actingMessage` | `String` | `"Wait..."` | Text to display inside the button when  a CRUD operation is in progress. |
| `inactiveMessage` | `String` | `"Clicked"` | Text to display inside the button when  `@click` callback can't be called |
| `animate` | `Boolean` | `true` | Use transitions for state change. Use velocity.js  |
| `buttonStyle`| `Object`| see [Styling Object](##✔-Vue-Crud-Button-Styling-Object) | Styling |
                               


## ✔ Vue-Crud-Button Styling Object

The styling object default is like this;

```
 {
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
    }
   ```
   
    
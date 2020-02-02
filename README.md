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

## 🔁 `Vue-Crud-Button` Props

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

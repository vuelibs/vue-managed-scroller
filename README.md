# vue-managed-scroller

[View live demo in sandbox](https://codesandbox.io/embed/v022jr1545)

## Installation
```
npm install --save vue-managed-scroller
```
or if you prefer yarn
```
yarn add vue-managed-scroller
```

## Usage
### Global
```
import Vue from 'vue';
import VueManagedScroller from 'vue-managed-scroller';

Vue.use(VueManagedScroller);
```
This will make *<managed-scroller>* and *<managed-scroller-shell>* available to all components within your Vue app.
### Local
```
import { ManagedScroller, ManagedScrollerShell } from 'vue-managed-scroller';

export default {
  ...
  components: {
    ManagedScroller,
    ManagedScrollerShell
  }
  ...
};
```

### Notes
- In order to handle resizing of scroller, either the `ResizeObserver` API or the `IntersectionObserver` API is used.  If neither of these are available a simple `resize` event is attached to the window.  If these APIs are not available, considering using an `IntersectionObserver` polyfill [here](https://github.com/w3c/IntersectionObserver/tree/master/polyfill).  Otherwsise you may consider handling resizing with your own logic via `resize` method.
- The scroller is set to a default of 100% width and 100% height, meaning the element wrapping the scroller should have a set width and height, otherwise you can set the width and height of the scroller manually via its props.

### Usage
You can simply create a basic `ManagedScroller` like this: (example using Vue's latest slot syntax)
```
<ManagedScroller :items="1000">
  <template v-slot="{ item }">
    <ManagedScrollerShell static>
      {{ item }}
    </ManagedScrollerShell>
  </template>
</ManagedScroller>
```
To manage shells with dynamically sized content use `updateShellSize` either from the `slot` or externally by using `ref` to the `ManagedScroller` component:
```
<ManagedScroller :items="1000" ref="managed-scroller">
  <template v-slot="{ item, updateShellSize }">
    <ManagedScrollerShell>
      <div @click="updateShellSize">
        {{ item }}
      </div>
    </ManagedScrollerShell>
  </template>
</ManagedScroller>
```
or externally such as:
```
this.$refs['managed-scroller'].updateShellSize(index);
```
A `ManagedScroller` may also contain multiple shells.  For example, in the event when a `'position: sticky;' item is used in conjuction with dynamically loaded content:
```
<ManagedScroller :items="items">
  <template v-slot="{ item, itemIndex, updateShellSize }">
    <ManagedScrollerShell static style="{ position: 'sticky' }">
      {{ itemIndex + 1 }}
    </ManagedScrollerShell>
    ....
    <ManagedScrollerShell>
      Dynamic content...
    </ManagedScrollerShell>
  </template>
</ManagedScroller>
```

### ManagedScroller
#### Configuration
| Property           | Type           | Default  | Description |
| --------           | ----           | -------  | ----------- |
| `items`            | Array, Number  | 0        | The items that will be rendered in the list. |
| `width`            | String, Number | 100%     | The width of the scroller area. |
| `height`           | String, Number | 100%     | The height of the scroller area. |
| `direction`        | String         | vertical | The scroll direction of the scroller: `vertical` or `horizontal`. |
| `buffer`           | Number         | 0        | How much content should be rendered outside the viewing area (in pixels). |
| `invertMouseWheel` | Boolean        | false    | Allow the usage of the mousewheel to scroll when the scroll `direction` is set to `horizontal`. |
#### Methods
| Method            | Description |
| ------            | ----------- |
| `resize`          | Triggers a resize of the scroller. |
| `updateShellSize` | Updates a shell size by an index. |
#### Events
| Event          | Description |
| -----          | ----------- |
| `startReached` | Triggers when the first item is rendered. |
| `endReached`   | Triggers when the last item is rendered. |
#### Default Slot
| Property          | Type     | Description |
| --------          | ----     | ----------- |
| `item`            | any      | A single item within the list.
| `itemIndex`       | Number   | The item index.
| `isVisible`       | Boolean  | Whether the rendered item is in the viewing area. |
| `updateShellSize` | Function | A method to update the shell size of a particular shell when the shell's size may have potentially changed. |

### ManagedScrollerShell
#### Configuration
| Property   | Type    | Default | Description |
| --------   | ----    | ------- | ----------- |
| `static`   | Boolean | false   | Whether the shell is a static shell or not. (Static shells are not wrapped by any divs and do not adjust in size via `updateShellSize`) |
| `size`     | Number  | 40      | The size of the shell; either the width or height depending on scroll `direction`. |

## License
This project is licensed under the MIT License - see the [LICENSE.md](https://github.com/levine404/vue-managed-scroller/blob/master/LICENSE) file for details.
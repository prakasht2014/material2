The `@angular/cdk/drag-drop` module provides you with a way to easily and declaratively create
drag-and-drop interfaces, with support for free dragging, sorting within a list, transferring items
between lists, animations, touch devices, custom drag handles, previews, and placeholders,
in addition to horizontal lists and locking along an axis.

### Getting started
Start by importing `DragDropModule` into the `NgModule` where you want to use drag-and-drop
features. You can now add the `cdkDrag` directive to elements to make them draggable. When
outside of a `cdkDropList` element, draggable elements can be freely moved around the page.
You can add `cdkDropList` elements to constrain where elements may be dropped.

<!-- example(cdk-drag-drop-overview) -->

### Reordering lists
Adding `cdkDropList` around a set of `cdkDrag` elements groups the draggables into a
reorderable collection. Items will automatically rearrange as an element moves. Note
that this will *not* update your data model; you can listen to the `cdkDropListDropped` event to
update the data model once the user finishes dragging.

<!-- example(cdk-drag-drop-sorting) -->

### Transferring items between lists
The `cdkDropList` directive supports transferring dragged items between connected drop zones.
You can connect one or more `cdkDropList` instances together by setting the `cdkDropListConnectedTo`
property.

<!-- example(cdk-drag-drop-connected-sorting) -->

Note that `cdkDropListConnectedTo` works both with a direct reference to another `cdkDropList`, or by
referencing the `id` of another drop container:

```html
<!-- This is valid -->
<div cdkDropList #listOne="cdkDropList" [cdkDropListConnectedTo]="[listTwo]"></div>
<div cdkDropList #listTwo="cdkDropList" [cdkDropListConnectedTo]="[listOne]"></div>

<!-- This is valid as well -->
<div cdkDropList id="list-one" [cdkDropListConnectedTo]="['list-two']"></div>
<div cdkDropList id="list-two" [cdkDropListConnectedTo]="['list-one']"></div>
```

### Attaching data
You can associate some arbitrary data with both `cdkDrag` and `cdkDropList` by setting `cdkDragData`
or `cdkDropListData`, respectively. Events fired from both directives include this data, allowing
you to easily identify the origin of the drag or drop interaction.

```html
<div cdkDropList [cdkDropListData]="list" *ngFor="let list of lists" (cdkDropListDropped)="drop($event)">
  <div cdkDrag [cdkDragData]="item" *ngFor="let item of list"></div>
</div>
```

### Styling
The `cdkDrag` and `cdkDropList` directive include only those styles strictly necessary for
functionality. The application can then customize the elements by styling CSS classes added
by the directives:

| Selector            | Description                                                              |
|---------------------|--------------------------------------------------------------------------|
| `.cdk-drop-list`         | Corresponds to the `cdkDropList` container.                                  |
| `.cdk-drag`         | Corresponds to a `cdkDrag` instance.                                     |
| `.cdk-drag-preview` | This is the element that will be rendered next to the user's cursor as they're dragging an item in a sortable list. By default the element looks exactly like the element that is being dragged. |
| `.cdk-drag-placeholder` | This is element that will be shown instead of the real element as it's being dragged inside a `cdkDropList`. By default this will look exactly like the element that is being sorted. |
| `.cdk-drop-dragging` | A class that is added to `cdkDropList` while the user is dragging an item.  |

### Animations
The drag-and-drop module supports animations both while sorting an element inside a list, as well as
animating it from the position that the user dropped it to its final place in the list. To set up
your animations, you have to define a `transition` that targets the `transform` property. The
following classes can be used for animations:

* `.cdk-drag` - If you add a `transition` to this class, it'll animate as the user is sorting
    through a list.
* `.cdk-drag-animating` - This class is added to a `cdkDrag` when the user has stopped dragging.
    If you add a `transition` to it, the CDK will animate the element from its drop position to
    the final position inside the `cdkDropList` container.

Example animations:

```css
/* Animate items as they're being sorted. */
.cdk-drop-dragging .cdk-drag {
  transition: transform 250ms cubic-bezier(0, 0, 0.2, 1);
}

/* Animate an item that has been dropped. */
.cdk-drag-animating {
  transition: transform 300ms cubic-bezier(0, 0, 0.2, 1);
}
```

### Customizing the drag area using a handle
By default, the user can drag the entire `cdkDrag` element to move it around. If you want to
restrict the user to only be able to do so using a handle element, you can do it by adding the
`cdkDragHandle` directive to an element inside of `cdkDrag`. Note that you can have as many
`cdkDragHandle` elements as you want:

<!-- example(cdk-drag-drop-handle) -->

### Customizing the drag preview
When a `cdkDrag` element is picked up, it will create a preview element visible while dragging.
By default, this will be a clone of the original element positioned next to the user's cursor.
This preview can be customized, though, by providing a custom template via `*cdkDragPreview`:

<!-- example(cdk-drag-drop-custom-preview) -->

### List orientation
The `cdkDropList` directive assumes that lists are vertical by default. This can be
changed by setting the `orientation` property to `"horizontal".

<!-- example(cdk-drag-drop-horizontal-sorting) -->

### Restricting movement along an axis
By default, `cdkDrag` allows free movement in all directions. To restrict dragging to a
specific axis, you can set `cdkDragLockAxis` on `cdkDrag` or `lockAxis` on `cdkDropList`
to either `"x"` or `"y"`.

<!-- example(cdk-drag-drop-axis-lock) -->

### Alternate drag root element
If there's an element that you want to make draggable, but you don't have direct access to it, you
can use the `cdkDragRootElement` attribute. The attribute works by accepting a selector and looking
up the DOM until it finds an element that matches the selector. If an element is found, it'll become
the element that is moved as the user is dragging. This is useful for cases like making a dialog
draggable.

<!-- example(cdk-drag-drop-root-element) -->

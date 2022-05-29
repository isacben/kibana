# Add Function To CircuitPython Core (C Programming)

The following resources need to be added to the source code in order to add a function to a module in CircuitPython.

### Update the shared-bindings code

In the main file (for example `shared-bindings/displayio/TileGrid.c`), add and define the function:

```c
STATIC mp_obj_t displayio_tilegrid_obj_contains(mp_obj_t self_in, mp_obj_t touch_tuple) {
    displayio_tilegrid_t *self = MP_OBJ_TO_PTR(self_in);

    mp_obj_t *touch_tuple_items;
    mp_obj_get_array_fixed_n(touch_tuple, 3, &touch_tuple_items);
    uint16_t x = 0;
    uint16_t y = 0;
    x = mp_obj_get_int(touch_tuple_items[0]);
    y = mp_obj_get_int(touch_tuple_items[1]);

    return mp_obj_new_bool(common_hal_displayio_tilegrid_contains(self, x, y));
}
MP_DEFINE_CONST_FUN_OBJ_2(displayio_tilegrid_contains_obj, displayio_tilegrid_obj_contains);
```

Down below in the same file, add the ROM Map Element:

```c
{ MP_ROM_QSTR(MP_QSTR_contains), MP_ROM_PTR(&displayio_tilegrid_contains_obj) },
```

Define the function in the .h file (for example `shared-bindings/displayio/TileGrid.h`):

```c
bool common_hal_displayio_tilegrid_contains(displayio_tilegrid_t *self, uint16_t x, uint16_t y);
```

### Update the shared-module code

This is the main functionality. Write the function in the source file (for example, `shared-module/displayio/TileGrid.c`):

```c
bool common_hal_displayio_tilegrid_contains(displayio_tilegrid_t *self, uint16_t x, uint16_t y) {
    uint16_t right_edge = self->x + (self->width_in_tiles * self->tile_width);
    uint16_t bottom_edge = self->y + (self->height_in_tiles * self->tile_height);
    if (x >= self->x && x <= right_edge &&
        y >= self->y && y <= bottom_edge) {
        return true;
    } else {
        return false;
    }
}
```
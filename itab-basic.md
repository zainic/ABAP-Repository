
## Instructions

Learn the basics about ABAP internal tables.

Your class has an internal table named `initial_data`. It has three columns: `GROUP`, `NUMBER`, and `DESCRIPTION`.

```abap
TYPES group TYPE c LENGTH 1.
TYPES: BEGIN OF initial_type,
         group       TYPE group,
         number      TYPE i,
         description TYPE string,
       END OF initial_type,
       initial_data TYPE STANDARD TABLE OF initial_type WITH EMPTY KEY.
```

### Step 1

Your first task is to complete the method `fill_itab` and place 6 records into this table with the following values:

| GROUP | NUMBER | DESCRIPTION |
| ----- | ------ | ----------- |
| A     | 10     | Group A-2   |
| B     | 5      | Group B     |
| A     | 6      | Group A-1   |
| C     | 22     | Group C-1   |
| A     | 13     | Group A-3   |
| C     | 500    | Group C-2   |

### Step 2

Next implement the method `add_to_itab` to add a record to the end of the internal table with following value:

| GROUP | NUMBER | DESCRIPTION |
| ----- | ------ | ----------- |
| A     | 19     | Group A-4   |

### Step 3

Now please sort the internal table in the method `sort_itab` with the `GROUP` column in alphabetical order and the `NUMBER` column in descending order.

### Step 4

In the method `search_itab`, search the sorted table and return the index of the record that has a `NUMBER` column value of 6.

---

## Solution :

```
CLASS zcl_itab_basics DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC .


  PUBLIC SECTION.
    TYPES group TYPE c LENGTH 1.
    TYPES: BEGIN OF initial_type,
             group       TYPE group,
             number      TYPE i,
             description TYPE string,
           END OF initial_type,
           itab_data_type TYPE STANDARD TABLE OF initial_type WITH EMPTY KEY.

    METHODS fill_itab
           RETURNING
             VALUE(initial_data) TYPE itab_data_type.

    METHODS add_to_itab
           IMPORTING initial_data TYPE itab_data_type
           RETURNING
            VALUE(updated_data) TYPE itab_data_type.

    METHODS sort_itab
           IMPORTING initial_data TYPE itab_data_type
           RETURNING
            VALUE(updated_data) TYPE itab_data_type.

    METHODS search_itab
           IMPORTING initial_data TYPE itab_data_type
           RETURNING
             VALUE(result_index) TYPE i.

  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.



CLASS zcl_itab_basics IMPLEMENTATION.
  METHOD fill_itab.
    initial_data = VALUE itab_data_type( ( group = 'A' number = 10 description = 'Group A-2' )
                            ( group = 'B' number = 5 description = 'Group B' ) 
                            ( group = 'A' number = 6 description = 'Group A-1' )
                            ( group = 'C' number = 22 description = 'Group C-1' )
                            ( group = 'A' number = 13 description = 'Group A-3' )
                            ( group = 'C' number = 500 description = 'Group C-2' ) ).
  ENDMETHOD.

  METHOD add_to_itab.
    updated_data = initial_data.
    INSERT VALUE #( group = 'A' number = 19 description = 'Group A-4' ) INTO TABLE updated_data.
  ENDMETHOD.

  METHOD sort_itab.
    updated_data = initial_data.
    SORT updated_data BY group ASCENDING number DESCENDING.
  ENDMETHOD.

  METHOD search_itab.
    DATA(temp_data) = initial_data.
    READ TABLE temp_data INTO DATA(test) WITH KEY number = 6.
    result_index = sy-tabix.
  ENDMETHOD.

ENDCLASS.

```

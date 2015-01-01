# Objective

A toy programming language based on things I like.

## Draft

| Objective | Description |
| --------- | ----------- |
| `{1, 2, 3, }` | An array. |
| `{'one' -> 1, 'two' -> 2, }` | An associative array (ordered dictionary). |
| `{1; 2; 3; }` | A set. |
| `{'one' -> 1; 'two' -> 2; }` | An associative set (unordered dictionary). |

Members of arrays are accessed like

```
numbers = {'1', '2', '3', }
three = numbers|2
```

Members of associative structures are accessed like

```
numbers = {'one' -> 1, 'two' -> 2, 'three' -> 3, }
three = numbers.'one'
```

Parentheses can be used for nested array access.

```
specialties = {
    'bob' -> 'ideas';
    'jim' -> 'implementation';
}

descriptions = {
    'ideas' -> 'thoughts';
    'implementation' -> 'work';
}

thoughts = descriptions.(specialties.'bob')
```

Functions are called like

```
[print:'hello world']
[mapFunction:someFunction overArray:someArray]
```

Methods are called like

```
game[startWithPlayer1:firstPlayer andPlayer2:secondPlayer]
```

## Sample code

What a program _might_ look like:

```
[makeGameWithWidth:width andHeight:height] ->
    game = {
        'width' -> width;
        'height' -> height;
        'board' -> null;
        [makeBoardWithWidth:width andHeight:height] ->
            board = []
            for y in [range:height]
                for x in [range:width]
                    [append:[randomCell] to:board]

        [makeBoard with game=self] ->
            game.'board' = game[makeBoardWithWidth:game.'width' andHeight:game.'height'];

        [gatherNeighborhoodAt:x andY:y with game=self] ->
            coordinates = {,}
            for yy in [range:y - 1 to:y + 1]
                for xx in [range:x - 1 to:x + 1]
                    if xx != x and yy != y
                        [appendValue:
                            {
                                [mod:xx for:game.'board'.width],
                                [mod:yy for:game.'board'.height],
                            }
                            to:coordinates
                        ]
            neighborhood = {,}
            for coordinate in coordinates
                [append:game.'board'|(coordinate|1)|(coordinate|0) to:neighborhood]
            return neighborhood;

        [stepNeighborhood:neighborhood withCell:cell] ->
            neighbors = [sum:neighborhood]
            if cell == 1
                if neighbors < 2 or neighbors > 3
                    return 0
                return 1
            elseIf neighbors == 3
                return 1
            return 0;

        [stepNeighborhoodAtX:x andY:y with game=self] =>
            neighborhood = game[gatherNeighborhoodAtX:x andY:y]
            game.'board'|y|x = game[stepNeighborhood:neighborhood withCell:game.'board'|y|x];

        [stepGame with game=self] =>
            for y in [range:game.'board'.'height']
                for x in [range:game.'board'.'width']
                    game[stepNeighborhoodAtX:x andY:y];
    }
    game[makeBoard]
    return game
```

## Things that are not detailed
* Flow control
* Keywords: if, else, null, true, false, etc.
* Dynamic dispatching
* Design patterns
* Regular expressions
* Accessors
* Programmatically manipulable function/method signatures
* Comments/documentation
* "Markers"
* Autocomplete
* Code folding
* Sets
* Named/multiple return values
* Keyword arguments
* Methods
* Constraints

## Things that are missing
* Namespacing
* Enumerations
* 'Tagged' functions/methods

## Things that are missing on purpose
* Inheritance
* Exceptions
* Programmatic specification

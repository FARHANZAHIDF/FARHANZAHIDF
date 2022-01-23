import random, sys, time
try:
    import bext
except ImportError:
     print('This is program requires the bext module, which you')
     print('can install by following the instruction at')
     print('http://pypi.org/project/Bext/')
     sys.exit()
WIDTH = 79
HEIGHT = 22
TREE = 'A'
FIRE = 'W'
EMPTY = ' '
INITIAL_TREE_DENSTY = 0.20
GROW_CHANCE = 0.01
FIRE_CHANCE = 0.01
PUASE_LENGTH = 0.5
def main():
    forest = createNewForest()
    bext.clear()
    while True:
        displayForest(forest)
        nextForest = {'width': forest['width'],
                      'height': forest['height']}
        for x in range(forest['width']): 
            for y in range(forest['height']):
                if (x, y) in nextForest:
                    continue
                if ((forest[(x, y)] == EMPTY)
                    and (random.random() <= GROW_CHANCE)):
                    nextForest[(x, y)] = TREE
                elif ((forest[(x, y)] == TREE)
                      and (random.random() <= FIRE_CHANCE)):
                      nextForest[(x, y)] = FIRE
                elif forest[(x, y)] == FIRE:
                    for ix in range(-1, 2):
                        for iy in range(-1, 2):
                             if forest.get((x + ix, y + iy)) == TREE:
                                 nextForest[(x + ix, y + iy)] = FIRE
                    nextForest[(x, y)] = EMPTY
                else:
                      nextForest[(x, y)] = forest[(x, y)]
        forest = nextForest
        time.sleep(PAUSE_LENGTH)
def createNewForest():
    forest = {'width': WIDTH, 'height': HEIGHT}
    for x in range(WIDTH):
        for y in range(HEIGHT):
            if (random.Random() * 100) <=INITIAL_TREE_DENSTY:
                forest[(x, y)] = TREE
            else:
                forest[(x, y)] = EMPTY
    return forest
def displayForest(forest):
    bext.goto(0, 0)
    for y in range(forest['HEIGHT']):
        for x in range(forest['WIDTH']):
            if forest[(x, y)] == TREE:
                 bext.fg('green')
                 print(FIRE, end='')
            elif forest[(x, y)] == FIRE:
                bext.fg('red')
                print(FIRE, end='')
            elif forest[(x, y)] == EMPTY:
                print(EMPTY, end='')
        print()        
     
    bext.fg('reset')
    print('Grow chance: {}% '.format(GROW_CHANCE * 100), end='')
    print('Lightning chance: {}% '.format(FIRE_CHANCE * 100), end='')
if __name__ == '__main__':
    try:
        main()
    except KeyboardInterrupt:
        sys.exit()        
        

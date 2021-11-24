==================================
Other
==================================

.. contents::
    :depth: 2

-------------------------------
1041. Robot Bounded In Circle
-------------------------------

On an infinite plane, a robot initially stands at (0, 0) and faces north. The robot can receive one of three instructions:

"G": go straight 1 unit;

"L": turn 90 degrees to the left;

"R": turn 90 degrees to the right.

The robot performs the instructions given in order, and repeats them forever.

Return true if and only if there exists a circle in the plane such that the robot never leaves the circle.

.. topic:: Example 1

    Input: instructions = "GGLLGG"

    Output: true

    Explanation: The robot moves from (0,0) to (0,2), turns 180 degrees, and then returns to (0,0).

    When repeating these instructions, the robot remains in the circle of radius 2 centered at the origin.

.. topic:: Example 2

    Input: instructions = "GG"

    Output: false

    Explanation: The robot moves north indefinitely.

.. topic:: Example 3

    Input: instructions = "GL"

    Output: true

    Explanation: The robot moves from (0, 0) -> (0, 1) -> (-1, 1) -> (-1, 0) -> (0, 0) -> ...

.. topic:: Constraints

    1 <= instructions.length <= 100

    instructions[i] is 'G', 'L' or, 'R'.

**Approach:** There are two circumstances that the robots will return to the starting point:

1. After it goes through one round of the instructions, it is at the origin. 
2. After it goes through one round of the instructions, it is not at the origin, but is facing DOWN, LEFT or RIGHT. If it is facing DOWN, then it only takes another round of instructions to make it go back to the origin. If it is facing LEFT or RIGHT, then it takes another three rounds of instructions to make ti go back to the origin. On the other hand, if it is facing UP, the robot can only go away from the origin infinitely. 

.. code-block:: java

    class Solution {
        enum Direction {
            LEFT,
            UP,
            RIGHT,
            DOWN
        }
        
        public boolean isRobotBounded(String instructions) {
            int[] currentLocation = new int[]{0, 0};
            Direction currentDirection = Direction.UP;
            
            for (int i=0; i<instructions.length(); i++) {
                char inst = instructions.charAt(i);
                if (inst == 'G') {
                    currentLocation = nextLocation(currentDirection, currentLocation[0], currentLocation[1]);
                    //System.out.println("inst: " + inst + " x: " + currentLocation[0] + " y: "+currentLocation[1] + " facing: "+currentDirection);
                } else {
                    currentDirection = nextDirection(currentDirection, inst);
                    //System.out.println("inst: " + inst + " x: " + currentLocation[0] + " y: "+currentLocation[1] + " facing: "+currentDirection);

                }
            }
                
            if (currentLocation[0] == 0 && currentLocation[1] == 0) {
                return true;
            } else if (currentDirection == Direction.UP) {
                return false;
            } else {
                return true;
            }
            
        }
        
        private int[] nextLocation(Direction direction, int currentX, int currentY) {
            int[] rst = new int[]{currentX, currentY};
            switch (direction) {
                case LEFT:  
                    rst[0] = currentX - 1;
                    break;
                case RIGHT:
                    rst[0] = currentX + 1;
                    break;
                case UP:  
                    rst[1] = currentY + 1;
                    break;
                case DOWN:
                    rst[1] = currentY - 1;
                    break;
                default:
                    break;
            }
            
            return rst;
        }
        
        private Direction nextDirection(Direction currentDirection, char instruction) {
            if (instruction == 'L') {
                switch (currentDirection) {
                        case LEFT:  
                        return Direction.DOWN;
                    case RIGHT:
                        return Direction.UP;
                    case UP:  
                        return Direction.LEFT;
                    case DOWN:
                        return Direction.RIGHT;
                    default:
                        return currentDirection;
                }
            }
            
            if (instruction == 'R') {
                switch (currentDirection) {
                        case LEFT:  
                        return Direction.UP;
                    case RIGHT:
                        return Direction.DOWN;
                    case UP:  
                        return Direction.RIGHT;
                    case DOWN:
                        return Direction.LEFT;
                    default:
                        return currentDirection;
                }
            }
            
            return currentDirection;
        }
    }

--------------------------------------
1344. Angle Between Hands of a Clock
--------------------------------------

Given two numbers, hour and minutes. Return the smaller angle (in degrees) formed between the hour and the minute hand.

.. topic:: Example 1

    Input: hour = 12, minutes = 30

    Output: 165

.. topic:: Example 2

    Input: hour = 3, minutes = 30

    Output: 75

.. topic:: Example 3

    Input: hour = 3, minutes = 15

    Output: 7.5

.. topic:: Example 4

    Input: hour = 4, minutes = 50

    Output: 155

.. topic:: Example 5

    Input: hour = 12, minutes = 0

    Output: 0
 
.. topic:: Constraints

    1 <= hour <= 12

    0 <= minutes <= 59

    Answers within 10^-5 of the actual value will be accepted as correct.

**Approach**: first calculate the degree between hour hand (exact hour) and minute hand, starting from hour hand, clock-wise, up to the minute hand. Then use this degree to minus the degree between hour hand actual position and the hour (for example for 12:30, the hour hand moves to half way between 12 and 1, so the movement is 15 degree).

.. code-block:: java

    public double angleClock(int hour, int minutes) {        
        double h = (hour==12)?0:hour;
        double m = (double)minutes/5;
        
        double diff;
        if (m>h) {
            diff = m-h;
        } else if (m<h){
            diff = 12-(h-m);
        } else {
            diff = 0;
        }
        
        double move = (minutes/60.0)*30.0;
        
        double rst = Math.abs(diff * 30 - move);
        
        return Math.min(360-rst, rst);
       
    }
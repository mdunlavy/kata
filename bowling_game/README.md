##The Story

We are going to create an object, BowlingGame, which, given a valid sequence of rolls for one line of American Ten-Pin Bowling, produces the total score for the game. This story is picked because it's about the right size for a couple of hours of Test-Driven Development demonstration. Here are some things that the program will not do:

We will not check for valid rolls.
We will not check for correct number of rolls and frames.
We will not provide scores for intermediate frames.
Depending on the application, this might or might not be a valid way to define a complete story, but we do it here for purposes of keeping the demonstration light. I think you'll see that improvements like those above would go in readily if they were needed.

I'll briefly summarize the scoring for this form of bowling:

Each game, or "line" of bowling, includes ten turns, or "frames" for the bowler.
In each frame, the bowler gets up to two tries to knock down all the pins.
If in two tries, he fails to knock them all down, his score for that frame is the total number of pins knocked down in his two tries.
If in two tries he knocks them all down, this is called a "spare" and his score for the frame is ten plus the number of pins knocked down on his next throw (in his next turn).
If on his first try in the frame he knocks down all the pins, this is called a "strike". His turn is over, and his score for the frame is ten plus the simple total of the pins knocked down in his next two rolls.
If he gets a spare or strike in the last (tenth) frame, the bowler gets to throw one or two more bonus balls, respectively. These bonus throws are taken as part of the same turn. If the bonus throws knock down all the pins, the process does not repeat: the bonus throws are only used to calculate the score of the final frame.
The game score is the total of all frame scores.
What makes this game interesting to score is the lookahead in the scoring for strike and spare. At the time we throw a strike or spare, we cannot calculate the frame score: we have to wait one or two frames to find out what the bonus is.

That should be all you need to know about the game. If, after reading this article, you remain confused, please let me know via email so that I can improve the writeup.

##The Design Idea

It seems to me that there are three main cases for each bowling Frame: the Strike, where the bowler knocks down all pins in one try, and gets ten points plus the sum of the next two throws; the Spare, where the bowler knocks down all ten pins in two tries, and gets ten points plus the value of the next throw; and the Open, where in two tries the bowler knocks down less than ten pins.

It seems like these Frame objects would each have a very simple Score() method, and that there would be no conditionals in the scoring code. One issue, however, is that all but the Open Frame need access to pin counts that are properly in a subsequent Frame. I feel sure that I could /design/ how this might work, but I want to see if I can build it in a test-driven fashion.

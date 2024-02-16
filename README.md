# Pacman_MultiAgents
Pacman games with multi agents. Evaluating the performance of Pacman and the ghosts.

<p align="justify"# --------------
# Licensing Information:  You are free to use or extend these projects for educational purposes provided that (1) you do not distribute or publish solutions, (2) you retain this notice, and (3) you provide clear
attribution to UC Berkeley, including a link to http://ai.berkeley.edu.<br/>
Attribution Information: The Pacman AI projects were developed at UC Berkeley. The core projects and autograders were primarily created by John DeNero (denero@cs.berkeley.edu) and Dan Klein (klein@cs.berkeley.edu). Student side autograding was added by Brad Miller, Nick Hay, and Pieter Abbeel (pabbeel@cs.berkeley.edu). </p>

<p><b>Task: Minimax Agent</b></p><br/>
<I>FUNCTION CODE:</I><br/>
class MinimaxAgent(MultiAgentSearchAgent):<br/>
def getAction(self, gameState):<br/>
def maxValue_fun(state, player_index, depth): <br/>
            if state.isWin() or state.isLose() or depth == self.depth:<br/>
                return scoreEvaluationFunction(state), None<br/>
            alpha = float('-inf')<br/>
            best_actn = None<br/>
            for i in state.getLegalActions(player_index):<br/>
                successor = state.generateSuccessor(player_index, i)<br/>
                next_index = (player_index+1) % state.getNumAgents()<br/>
                val = minValue_fun(successor, next_index, depth)<br/>
                if val > alpha:<br/>
                    alpha = val<br/>
                    best_actn = i<br/>
            return alpha, best_actn<br/>
def minValue_fun(state, player_index, depth):<br/>
            if state.isWin() or state.isLose() or depth == self.depth:<br/>
                return scoreEvaluationFunction(state)<br/>
            beta = float('inf')<br/>
            for j in state.getLegalActions(player_index):<br/>
                successor = state.generateSuccessor(player_index, j)<br/>
                next_index = (player_index+1) % state.getNumAgents()<br/>
                if next_index == 0:<br/>
                    beta = min(beta, maxValue_fun(successor, next_index, depth+1) [0])<br/>
                else:<br/>
                    beta = min(beta, minValue_fun(successor, next_index, depth))<br/>
            return beta<br/>

 bst_score, bst_actn = maxValue_fun(gameState, 0, 0)<br/>
 return bst_actn<br/>
<p>------------------------------------------------------------------------------------</p>
<p><b>COMMENTS:</b></p>
<p align="justify">We have implemented two functions, maxValue_fun and minValue_fun for the required minimax problem. The maxValue_fun function helps maximize Pacman’s turn in the game as part of the minimax algorithm. The maxValue_fun identifies and iterates over all the legal actions possible by making a function call to the minValue_fun function. The alpha value and best action variables are regularly updated if a higher value is found.The minValue_fun minimizes the player’s turn in the game sequence (here ghosts). It goes over all possible actions for the current state and computes the value by recursively either calling the maxValue_fun (if the next player is Pacman) or itself (if the next player is the ghost). Ultimately, it returns the minimum value found for the current state.
Finally, we pass the gameState and the root node to the maxValue_fun to obtain the best action for Pacman.</p>
<p align="justify"><b>How does the “max” function work?</b><br/>
The max function essentially helps obtain the maximum value and the best action for Pacman to take. If the current state examined is the win state/loss state or if we have reached the maximum depth, then we just return the score for that state. The function examines all possible legal actions for the player and calls the min for each successor that is generated. It keeps track of the maximum value and ultimately returns it.</p>
<p align="justify"><b>How does the “min” function work?</b><br/>
Like the max function, the min function also examines each possible action that can be taken from the current state. The function makes recursive calls to the max function or itself based on the next player move. It keeps track and returns the minimum value.
<p align="justify"><b>How to recursively call the “max” function and “min” function?</b><br/> 
Both functions make recursive calls to each other/themselves respectively based on the move in the minimax algorithm.<br/>
In function max:<br/>
val = minValue_fun(successor, next_index, depth)<br/>
The max function calls min to minimize the opponents’ (ghosts’) moves.<br/>
In function min:<br/>
<b>if next_index == 0:<br/>
             beta = min(beta, maxValue_fun(successor, next_index, depth+1) [0])<br/>
else:<br/>
             beta = min(beta, minValue_fun(successor, next_index, depth))</b><br/>
Min function calls max to maximize Pacman’s moves. It calls itself when the move is to be played by the ghosts.</p>
<p align="justify"><b>How is depth=4 reached? </b><br/>
We increment the depth during a recursive call. The depth will be incremented until the desired depth is reached. Once we reach the desired depth, we evaluate the score at that state.
min(beta, maxValue_fun(successor, next_index, depth+1) [0])</p>
<p align="justify"><b>How does the root node return the best action?</b><br/>
bst_score, bst_actn = maxValue_fun(gameState, 0, 0)<br/>
We call the max function on the initial game state, the starting index of Pacman and depth 0. It will assist the minimax agent to return the best action for Pacman.</p>
<p align="justify"><b>How do you set the PlayerIndex for Pacman and for three ghosts?</b><br/>
The player_index and next_index variable helps keep track of the turn of Pacman and the ghosts. Pacman’s player index is set to 0. The index for each ghost is determined by the number of agents in the game.
When depth<4, how to go from the 3rd ghost to the next player-MAX, and at the same time increase the depth by 1?<br/>
<b>if next_index == 0:<br/>
             beta = min(beta, maxValue_fun(successor, next_index, depth+1) [0])<br/>
else:<br/>
             beta = min(beta, minValue_fun(successor, next_index, depth))</b><br/>
The above code snippet will determine if the player after the current ghost is Pacman. If it is Pacman, we call max with an incremented depth value. Basically, we are maximizing Pacman’s turn. This increases the depth by 1 while we move from the 3rd ghost to P.</p>

<!DOCTYPE html>
<html lang="en"><head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>assignment day3</title>
</head>
<body>
    <font size="6">
        <b>Backtracking</b>
     </font><br>
         <font>Copyright ©2002 by David Matuszek</font>  
    <p>Backtracking is a form of recursion.</p>
    <p>The usual scenario is that you are faced with a number of options, and you must choose one of these. After you make your choice you will get a new set of options; just what set of options you get depends on what choice you made. This procedure is repeated over and over until you reach a final state. If you made a good sequence of choices, your final state is a <i>goal state;</i>  if you didn't, it isn't.</p>
    <p>Conceptually, you start at the root of a tree; the tree probably has some good leaves and some bad leaves, though it may be that the leaves are all good or all bad. You want to get to a good leaf. At each node, beginning with the root, you choose one of its children to move to, and you keep this up until you get to a leaf.</p>
    <p>Suppose you get to a bad leaf. You can <i>backtrack</i> to continue the search for a good leaf by revoking your <i>most recent</i> choice, and trying out the next option in that set of options. If you run out of options, revoke the choice that got you here, and try another choice at that node. If you end up at the root with no options left, there are no good leaves to be foun</p>
    <p>This needs an example.</p>
    <center>
        <img src="treesearch.gif" alt="404 error">
    </center>
    <ol type="1">
        <li>Starting at Root, your options are A and B</li>
        <li> You choose A. At A, your options are C and D. You choose C.</li>
        <li> C is bad. Go back to A. At A, you have already tried C, and it failed. Try D.</li>
        <li> D is bad. Go back to A.</li>
        <li> At A, you have no options left to try. Go back to Root.</li>
        <li> Root, you have already tried A. Try B</li>
        <li> B, your options are E and F. Try E.</li>
        <li> E is good. Congratulations!</li>
    </ol>
    <p>In this example we drew a picture of a tree. The tree is an abstract model of the possible sequences of choices we could make. There is also a data structure called a tree, but usually we don't have a data structure to tell us what choices we have. (If we do have an actual tree data structure, backtracking on it is called<i> depth-first tree searching.)</i></p>
    <h3>The backtracking algorithm.</h3>
    <p>Here is the algorithm (in pseudocode) for doing backtracking from a given node n:</p>
    <pre style="color:brown;font-size:16px;">        boolean solve(Node n) {
            if n is a leaf node {
                if the leaf is a goal node, return true
                else return false
            } else {
                for each child c of n {
                    if solve(c) succeeds, return true
                }
                return false
            }
        }
        
    </pre>
    <p>Notice that the algorithm is expressed as a <i>boolean</i> function. This is essential to understanding the algorithm. <font color="brown" size="4">If solve(n)</font> true, that means node n is part of a solution--that is, node n is one of the nodes on a path from the root to some goal node. We say that n is<i> solvable</i>.<font color="brown" size="4">solve(n) </font> false, then there is no path that includes n to any goal node</p>
    <p>How does this work?</p>
    <ul>
        <li>If any child of<font color="brown" size="4"> n</font> is solvable,then n is solvable</li>
        <li>If no child of <font color="brown" size="4"> n</font> is solvable, then n is not solvable.</li>
    </ul>
    <p>Hence, to decide whether any non-leaf node n is solvable (part of a path to a goal node), all you have to do is test whether any child of n is solvable. This is done recursively, on each child of n. In the above code, this is done by the lines</p>
    <pre style="color:brown;font-size:16px"> 
        for each child c of n {
            if solve(c) succeeds, return true
        }
        return false
        
    </pre>
    <p>Eventually the recursion will "bottom" out at a leaf node. If the leaf node is a goal node, it is solvable; if the leaf node is not a goal node, it is not solvable. This is our base case. In the above code, this is done by the lines</p>
    <pre style="color:brown;font-size:16px">     if n is a leaf node {
         if the leaf is a goal node, return true
         else return false
        }
    </pre>
    <p>The backtracking algorithm is simple but important. You should understand it thoroughly. Another way of stating it is as follows:</p>
    <ul>
        <li>To search a tree:</li><br>
    <ol type="1">
        <li>If the tree consists of a single leaf, test whether it is a goal node,</li>
        <li>Otherwise,search the subtrees until you find one containing a goal node, or until you have searched them all unsuccessfully.</li>
    </ol>
    </ul>
    <h3>Non-recursive backtracking, using a stack</h3>
    <p>Backtracking is a rather typical recursive algorithm, and any recursive algorithm can be rewritten as a stack algorithm. In fact, that is how your recursive algorithms are translated into machine or assembly language</p>
    <pre style="color:brown;font-size:16px">        boolean solve(Node n) {
            put node n on the stack;
            while the stack is not empty {
                if the node at the top of the stack is a leaf {
                    if it is a goal node, return true
                    else pop it off the stack
                }
                else {
                    if the node at the top of the stack has untried children
                        push the next untried child onto the stack
                    else pop the node off the stack
            
            }
            return false
        }
        
    </pre>
    <p>Starting from the root, the only nodes that can be pushed onto the stack are the children of the node currently on the top of the stack, and these are only pushed on one child at a time; hence, the nodes on the stack at all times describe a valid path in the tree. Nodes are removed from the stack only when it is known that they have no goal nodes among their descendents. Therefore, if the root node gets removed (making the stack empty), there must have been no goal nodes at all, and no solution to the problem.</p>
    <p>When the stack algorithm terminates successfully, the nodes on the stack form (in reverse order) a path from the root to a goal node</p>
    <p>Similarly, when the recursive algorithm finds a goal node, the path information is embodied (in reverse order) in the sequence of recursive calls. Thus as the recursion unwinds, the path can be recovered one node at a time, by (for instance) printing the node at the current level, or storing it in an array.</p>
    <p>Here is the recursive backtracking algorithm, modified slightly to print (in reverse order) the nodes along the successful path:</p>
    <pre style="color:brown;font-size:16px">        boolean solve(Node n) {
         if n is a leaf node {
             if the leaf is a goal node {
                 print n
                 return true
             }
                 else return false
            } else {
                for each child c of n {
                    if solve(c) succeeds {
                        print n
                        return true
                    }
                }
                return false
            }
        }

    </pre>
    <h3>Keeping backtracking simple</h3>
    <p>All of these versions of the backtracking algorithm are pretty simple, but when applied to a real problem, they can get pretty cluttered up with details. Even determining whether the node is a leaf can be complex: for example, if the path represents a series of moves in a chess endgame problem, the leaves are the checkmate and stalemate solutions.</p>
    <p>To keep the program clean, therefore, tests like this should be buried in methods. In a chess game, for example, you could test whether a node is a leaf by writing a <font color="brown" size="4">gameOver method</font> (or you could even call it<font color="brown" size="4">isLeaf</font>).This method would encapsulate all the ugly details of figuring out whether any possible moves remain.</p>
    <p>Notice that the backtracking altorithms require us to keep track, for each node on the current path, which of its children have been tried already (so we don't have to try them again). In the above code we made this look simple, by just saying for <font color="brown" size="4">each child c of n</font>. In reality, it may be difficult to figure out what the possible children are, and there may be no obvious way to step through them. In chess, for example, a node can represent one arrangement of pieces on a chessboard, and each child of that node can represent the arrangement after some piece has made a legal move. How do you find these children, and how do you keep track of which ones you've already examined?</p>
    <p>The most straightforward way to keep track of which children of the node have been tried is as follows: Upon initial entry to the node (that is, when you first get there from above), make a list of all its children. As you try each child, take it off the list. When the list is empty, there are no remaining untried children, and you can return "failure." This is a simple approach, but it may require quite a lot of additional work.</p>
    <p>There is an easier way to keep track of which children have been tried, <b>if</b> you can define an ordering on the children. If there is an ordering, and you know which child you just tried, you can determine which child to try next.</p>
    <p>For example, you might be able to number the children<font color="brown" size="4"> 1 n</font>, and try them in numerical order. Then, if you have just tried child<font color="brown" size="4"> k</font>, you know that you have already tried children <font color="brown" size="4">k-1</font>, and you have not yet tried children <font color="brown" size="4">k+1</font>, through <font color="brown " size="4">n</font>. Or, if you are trying to color a map with just four colors, you can always try red first, then yellow, then green, then blue. If child yellow fails, you know to try child green next. If you are searching a maze, you can try choices in the order left, straight, right (or perhaps north, east, south, west).</p>
    <p>It isn't always easy to find a simple way to order the children of a node. In the chess game example, you might number your pieces (or perhaps the squares of the board) and try them in numerical order; but in addition each piece may also have several moves, and these must also be ordered.</p>
    <p>You can probably find some way to order the children of a node. If the ordering scheme is simple enough, you should use it; but if it is too cumbersome, you are better off keeping a list of untried children.</p>
    <h3>Example: TreeSearch</h3>
    <p>For starters, let's do the simplest possible example of backtracking, which is searching an actual tree. We will also use the simplest kind of tree, a binary tree.</p>
    <p>For starters, let's do the simplest possible example of backtracking, which is searching an actual tree. We will also use the simplest kind of tree, a binary tree.</p>
    <p>A <b>binary tree </b> is a data structure composed of node. One <b>node</b> is designed as the<b>root</b> node. Each node can reference (point to) zero, one,or, teo other nides, which are called its<b>children</b> . the children are refered to as the <b>left child</b> and/or <b>rihgt child</b>. All nodes are reachable (by one or more steps) from the root node, and there are no cycles. For our purposes, although this is not part of the definition of a binary tree, we will say that a node might or might not be a goal node, and will contain its name. The first example in this paper (which we repeat here) shows a binary tree.</p>
    <center>
        <img src="treesearch.gif" alt="404 error">
    </center>
    <p>Here's a definition of the BinaryTree class:</p>
    <pre style="color:brown;font-size:16px">     public class BinaryTree {
        BinaryTree leftChild = null;
        BinaryTree rightChild = null;
        boolean isGoalNode = false;
        String name;
            
        BinaryTree(String name, BinaryTree left, BinaryTree right, boolean isGoalNode) {
            this.name = name;
            leftChild = left;
            rightChild = right;
            this.isGoalNode = isGoalNode;
        }
    }
    </pre>
    <p>next we will create a <font color="brown" size="4">TreeSearch</font> class, and in it we will define a method <font color="brown" size="4">makeTree</font> which constructs the above binary tree.</p>
    <pre style="color:brown;font-size:16px">        static BinaryTree makeTree() {
            BinaryTree root, a, b, c, d, e, f;
            c = new BinaryTree("C", null, null, false);
            d = new BinaryTree("D", null, null, false);
            e = new BinaryTree("E", null, null, true);
            f = new BinaryTree("F", null, null, false);
            a = new BinaryTree("A", c, d, false);
            b = new BinaryTree("B", e, f, false);
            root = new BinaryTree("Root", a, b, false);
            return root;
        }
        
    </pre>
    <p>Here's a main program to create a binary tree and try to solve it:</p>
    <pre style="color:brown;font-size:16px">        public static void main(String args[]) {
            BinaryTree tree = makeTree();
            System.out.println(solvable(tree));
        }
    </pre>
    <p>And finally, here's the recursive backtracking routine to "solve" the binary tree by finding a goal node.</p>
    <pre style="color:brown;font-size:16px">        static boolean solvable(BinaryTree node) {
    <span style="color:lightseagreen;">/* 1 */</span>if (node == null) return false;
    <span style="color:lightseagreen;">/* 2 */</span> if (node.isGoalNode) return true;
    <span style="color:lightseagreen;">/* 3 */</span> if (solvable(node.leftChild)) return true;
    <span style="color:lightseagreen;">/* 4 */</span> if (solvable(node.rightChild)) return true;
    <span style="color:lightseagreen;">/* 5 */</span>  return false;
                }
    </pre>
    <p>Here's what the numbered lines are doing:</p>
    <ol type="1">
        <li>If we are given a null<font color="brown" size="4"> node</font>, it's not solvable. This statement is so that we can call this method with the children of a node, without first checking whether those children actually exist.</li>
        <li>If the node we are given is a goal node, return success.</li>
        <li>See if the left child of<font color="brown" size="4"> node </font>is solvable, and if so, conclude that<font color="brown" size="4"> node</font> is solvable. We will only get to this line if <font color="brown" size="4">node </font>is non-null and is not a goal node, says to</li>
        <li>Do the same thing for the right child.</li>
        <li>Since neither child of <font color="brown" size="4">node</font> is solvable, <font color="brown" size="4">node</font> itself is not solvable.</li>

    </ol>
    <p>This program runs correctly and produces the unenlightening result<font color="brown" size="4"> true</font></p>
    <p>Each time we ask for another node, we have to check if it is null. In the above we put that check as the first thing <font color="brown" size="4">insolvable</font>. An alternative would be to check first whether each child exists, and recur only if they do. Here's that alternative version:</p>
    <pre style="color:brown;font-size:16px">     static boolean solvable(BinaryTree node) {
         if (node.isGoalNode) return true;
         if (node.leftChild != null &amp;&amp; solvable(node.leftChild)) return true;
         if (node.rightChild != null &amp;&amp; solvable(node.rightChild)) return true;
         return false;
        }
        
    </pre>
    <p>I think the first version is simpler, but the second version is slightly more efficient.</p>
    <br>
    <h3>What are the children?</h3>
    <p>One of the things that simplifies the above binary tree search is that, at each choice point, you can ignore all the previous choices. Previous choices don't give you any information about what you should do next; as far as you know, both the left and the right child are possible solutions. In many problems, however, you may be able to eliminate children immediately, without recursion.</p>
    <p>Consider, for example, the problem of four-coloring a map. It is a theorem of mathematics that any map on a plane, no matter how convoluted the countries are, can be colored with at most four colors, so that no two countries that share a border are the same color.</p>
    <p>To color a map, you choose a color for the first country, then a color for the second country, and so on, until all countries are colored. There are two ways to do this:</p>
    <ul style="list-style-type:none">
        <li style="margin-left:15px;"><strong>Method 1.</strong>Try each of the four possible colors, and recur. When you run out of countries, check whether you are at a goal node. </li>  
          <li style="margin-left:15px;"><strong>Method 2.</strong> Try only those colors that have not already been used for an adjacent country, and recur. If and when you run out of countries, you have successfully colored the map.
          </li>
        </ul>      
            <p>
                Let's apply each of these two methods to the problem of coloring a checkerboard. This should be easily solvable; after all, a checkerboard only needs two colors.
            </p>
            <p>
                In both methods, the colors are represented by integers, from<font color="brown" size="4"> RED=1 </font>to<font color="brown" size="4"> BLUE=4</font>. We define the following helper methods. The helper method code isn't displayed here because it's not important for understanding the method that does the backtracking.
            </p>
            <dl>
                <dt>
                  <font color="brown" size="4">boolean mapIsOK()</font>
                </dt>
                <dd>
                    Used by method 1 to check (at a leaf node) whether the entire map is colored correctly.
                </dd><br>
                <dt>
                   <font color="brown" size="4"> boolean okToColor(int row, int column, int color)</font>
                </dt>
                <dd>
                    Used by method 2 to check, at every node, whether there is an adjacent node already colored with the given color.</dd><br>
                    <dt>
                        int[] nextRowAndColumn(int row, int column)
                    </dt>
                    <dd>
                        Used by both methods to find the next "country" (actually, the row and column of the next square on the checkerboard).
                    </dd>

                
            </dl>
            <p>
                Here's the code for method 1:
            </p>
  <pre style="color: brown;font-size:16px">    boolean explore1(int row, int column, int color) {
        if (row &gt;= NUM_ROWS)<font color="darkred" size="4"><b> return mapIsOK()</b></font>;
        map[row][column] = color;
        for (int nextColor = RED; nextColor &lt;= BLUE; nextColor++) {
            int[] next = nextRowAndColumn(row, column);
            if (explore1(next[0], next[1], nextColor)) return true;
        }
        return false;
    }
  </pre>
  <p>And here's the code for method 2:</p>
  <pre style="color: brown;font-size:16px">    boolean explore2(int row, int column, int color) {
        if (row &gt;= NUM_ROWS)<font color="darkred" size="4"> <b> true</b></font>;
     <font color="darkred" size="4"> <b>  if (okToColor(row, column, color))</b></font> {
            map[row][column] = color;
            for (int nextColor = RED; nextColor &lt;= BLUE; nextColor++) {
                int[] next = nextRowAndColumn(row, column);
                if (explore2(next[0], next[1], nextColor)) return true;
            }
        }
        return false;
    }
  </pre>
  <p>
    Those appear pretty similar, and you might think they are equally good. However, the timing information suggests otherwise:
  </p>
  <table border="1" align="center">
    <tbody>
       
        <tr>
            <td></td>
            <td bgcolor="silver ">2 by 3 map </td>
            <td bgcolor="silver">3 by 3 map</td>
            <td bgcolor="silver "> 3 by 4 map</td>
        </tr>
        <tr>
            <td bgcolor="silver">Method 1:</td>
            <td>60 ms.</td>
            <td>940 ms.</td>
            <td>60530 ms.(1 minute)</td>
        </tr>
        <tr>
            <td bgcolor="silver">Method 2:</td>
            <td>0 ms.</td>
            <td>0 ms.</td>
            <td>0 ms.</td>
        </tr>
    </tbody>
  </table>
  <p>
    The zeros in the above table indicate times too short to measure (less than 1 millisecond). Why this huge difference? Either of these methods could have exponential growth. Eliminating a node automatically eliminates all of its descendents, and this will often prevent exponential growth. Conversely, by waiting to check until a leaf node is reached, exponential growth is practically guaranteed.<b> If there is any way to eliminate children (reduce the set of choices), do so!</b>
  </p>
  <h3>Debugging techniques</h3>
  <p>
    Often our first try at a program doesn't work, and we need to debug it. Debuggers are helpful, but sometimes we need to fall back on inserting print statements. There are some simple tricks to making effective use of print statements. These tricks can be applied to any program, but are especially useful when you are trying to debug recursive routines.
  </p>
  <p>
   <b>Trick #1: Indent when you print method entries and exits.</b>  Often, the best debugging technique is to print every method call and return (or at least the most important ones). You probably want to print, for each method, what parameters it came in with, and what value it leaves with. However, if you just print a long list of these, it's hard to match up method exits with their corresponding entries. Indenting to show the level of nesting can help.
  </p>
  <p>
    <b>Trick #2: Use specialized print methods for debugging.</b> Don't clutter up your actual code more than you must. Also, remember that code inserted for debugging purposes can itself contain bugs, or (in the worst case) can affect the results, so be very careful with it
  </p>
  <p>
    Here's our debugging code. For this trivial program, there's almost more debugging code than actual code, but in larger programs the proportions will be better.
  </p>
  <pre style="color: brown;font-size:16px">    static String name(BinaryTree node) {
        if (node == null) return null;
        else return node.name;
    }
    static void enter(BinaryTree node) {
        System.out.println(indent + "Entering solvable(" + name(node) + ")");
        indent = indent + "|  ";
    }
    static boolean yes(BinaryTree node) {
        indent = indent.substring(3);
        System.out.println(indent + "solvable(" + name(node) + ") returns true");
        return true;
    }
    static boolean no(BinaryTree node) {
        indent = indent.substring(3);
        System.out.println(indent + "solvable(" + name(node) + ") returns false");
        return false;
    }
  </pre>
  <p>
    To use this code, we modify solvable as follows:
  </p>
  <pre style="color: brown;font-size:16px">    static boolean solvable(BinaryTree node) {
        <font color="brown" size="4"><b>enter(node);</b> </font>
        if (node == null) return <font color="brown" size="4"><b>no(node);</b></font>
        if (node.isGoalNode) return <font color="brown" size="4"><b>yes(node);</b></font>
        if (solvable(node.leftChild)) return <font color="brown" size="4"><b>yes(node);</b></font>
        if (solvable(node.rightChild)) return<font color="brown" size="4"><b> yes(node);</b></font>
        return <font color="brown" size="4"><b>no(node);</b></font>
    }
  </pre>
  <p>
    And we get these results:
  </p>
  <pre style="color: black;font-size:16px">   <b>Entering solvable(Root)
    |  Entering solvable(A)
    |  |  Entering solvable(C)
    |  |  |  Entering solvable(null)
    |  |  |  solvable(null) returns false
    |  |  |  Entering solvable(null)
    |  |  |  solvable(null) returns false
    |  |  solvable(C) returns false
    |  |  Entering solvable(D)
    |  |  |  Entering solvable(null)
    |  |  |  solvable(null) returns false
    |  |  |  Entering solvable(null)
    |  |  |  solvable(null) returns false
    |  |  solvable(D) returns false
    |  solvable(A) returns false
    |  Entering solvable(B)
    |  |  Entering solvable(E)
    |  |  solvable(E) returns true
    |  solvable(B) returns true
    solvable(Root) returns true
    true</b>
  </pre>
  <p>
    <b>Trick #3: Never discard your debugging statements.</b> Writing debugging statements is programming, too. Often it's as much work to debug the debugging statements as it is to debug the actual program. Once your program is working, why throw this code away?
  </p>
  <p>
    Obviously, you don't want to print out all this debugging information from a program you are ready to submit (or to turn over to your manager). You could comment out your debugging calls, but that can be a lot of work. What's more, in the above example, you would have to replace every<font color="brown" size="4"> return(yes(node))</font> with <font color="brown" size="4">return(true)</font>, and every <font color="brown" size="4">return(no(node))</font> with <font color="brown" size="4">return(false).</font> With all these changes, you might introduce new bugs into your program.
  </p>
  <p>
    The simple solution is to make your debugging statements conditional. For example,
  </p>
    <pre style="color: brown;font-size:16px">        <font color="brown" size="4"> <b>static final boolean debugging = false;
        </b></font>
        static void enter(BinaryTree node) {
            <font color="brown" size="4"><b> if (debugging) {</b></font>
                System.out.println(indent + "Entering solvable(" + name(node) + ")");
                indent = indent + "|  ";
            }
        }
        static boolean yes(BinaryTree node) {
            <font color="brown" size="4"><b>if (debugging) {</b></font>
                indent = indent.substring(3);
                System.out.println(indent + "solvable(" + name(node) + ") returns true");
            }
            return true;
        }
        static boolean no(BinaryTree node) {
            <font color="brown" size="4"><b>if (debugging) {</b></font>
                indent = indent.substring(3);
                System.out.println(indent + "solvable(" + name(node) + ") returns false");
           <b> }</b>
                return false;
        }
        </pre>
        <p>
            In industry, actual programs often have multiple flags to control different aspects of debugging. Don't worry too much about making your code larger; modern compilers will notice that since the variable <font color="brown" size="4">dibugging is final</font> , and the controlled code will be discarded.
        </p>
        <p>
           <b> Trick #4: Create an Exception</b>. If an Exception is thrown, you can get information about just where it happened by sending it the message<font color="brown" size="4">printStackTrace(PrintStream)</font> . Since an Exception is an object like any other, you can create and throw your own Exceptions. However, Java programmers don't always realize that you can create an Exception <i>without</i> throwing it. For example, the following code
        </p>
        <p>
            <font color="brown" size="4"> new Exception("Checkpoint Charlie").printStackTrace(System.out);</font>
        </p>
        <p>
            will print out a message something like this, and the program will then continue normally. That is, the above code just acts like a print statement.
        </p>
          <pre style="color: brown;font-size:16px">  java.lang.Exception: Checkpoint Charlie
        at TreeSearch.solvable(TreeSearch.java:53)
        at TreeSearch.solvable(TreeSearch.java:57)
        at TreeSearch.main(TreeSearch.java:72)
        at __SHELL38.run(__SHELL38.java:16)
        at bluej.runtime.ExecServer.suspendExecution(Unknown Source)
    
          </pre>
          <h3>Example: Cindy's Puzzle</h3>
          <p>
            I call the following puzzle "Cindy's puzzle" for historical reasons. You have some number n of black marbles and the same number of white marbles, and you have a playing board which consists simply of a line of <font color="brown" size="4"> 2n+1 </font>spaces to put the marbles in. Start with the black marbles all at one end (say, the left), the white marbles all at the other end, and a free space in between.
          </p>
          <table border="1" align="center" cellspacing="0" cellpadding="8">
            <tbody>
                <tr bgcolor="silver">
                    <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/black-ball.gif"></td>
                    <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/black-ball.gif"></td>
                    <td width="44"> &nbsp; </td>
                    <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/white-ball.gif"></td>
                    <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/white-ball.gif"></td>    
                </tr>
            </tbody>
          </table>
          <p>
            The goal is to reverse the positions of the marbles:
          </p>
          <table>
            </table><table border="1" cellspacing="0" cellpadding="8" align="center">
                <tbody><tr bgcolor="silver">
                    <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/white-ball.gif"></td>
                    <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/white-ball.gif"></td>
                    <td width="44">&nbsp;</td>
                    <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/black-ball.gif"></td>
                   <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/black-ball.gif"></td>  
                </tr>
          </tbody></table>
          <p>
            The black marbles can only move to the right, and the white marbles can only move to the left (no backing up). At each move, a marble can either:
          </p>
          <ul>
            <li>Move one space ahead, if that space is clear, or</li>
            <li>Jump ahead over exactly one marble of the opposite color, if the space just beyond that marble is clear.</li>
          </ul>
          <p>
            For example, you could make the following sequence of moves:
          </p>
          <table align="center" border="0" cellpadding="8" cellspacing="0">
            <tbody>
            <tr>
                <td>
                    <b>Starting position:</b>
                </td>
        
                <td>
                    <table border="1" cellspacing="0" cellpadding="6" align="center">
                        <tbody>
                            <tr bgcolor="silver">
                                <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/black-ball.gif"></td>
                                <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/black-ball.gif"></td>
                                <td width="44"> &nbsp;</td>
                                <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/white-ball.gif"></td>
                                <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/white-ball.gif"></td>       
                            </tr>
                        </tbody>
                    </table> 
                </td>
            </tr>
            <tr>
                <td>
                    <b>Black moves ahead:</b>
                </td>
                <td>
        
                    <table border="1" cellspacing="0" cellpadding="6" align="center">
                    <tbody>
                        <tr bgcolor="silver">
                            <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/black-ball.gif"></td>
                             <td width="44"> &nbsp;</td>
                             <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/black-ball.gif"></td>
                             <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/white-ball.gif"></td>
                             <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/white-ball.gif"></td>  
                        </tr>
                    </tbody>
                    </table>
                </td>
        </tr>
        
            <tr>
                <td><b>White jumps:</b></td>
                <td>
                    <table border="1" cellspacing="0" cellpadding="6" align="center">
                        <tbody>
                        <tr bgcolor="silver">
                            <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/black-ball.gif"></td>
              
                            <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/white-ball.gif"></td>
                            <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/black-ball.gif"></td>
                            <td width="44">&nbsp;</td>
                        <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/white-ball.gif"></td>    
                        </tr>
                    </tbody>
                    </table>
                </td>
            </tr>
        
            <tr>
                <td><b>Black moves ahead:</b></td>
                <td>
                    <table border="1" cellspacing="0" cellpadding="6" align="center">
                    <tbody>
                        <tr bgcolor="silver">
                          <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/black-ball.gif"></td>
                          <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/white-ball.gif"></td>
                          <td width="44"> &nbsp;</td>
                          <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/black-ball.gif"></td>
                          <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/white-ball.gif">
                         </td>       
                        </tr>
                    </tbody>
                    </table>
                </td>
            </tr>
        
            <tr>
                <td><b>Black jumps:</b></td>
                <td>
                    <table border="1" cellspacing="0" cellpadding="6" align="center">
                    <tbody>
                        <tr bgcolor="silver">
                            <td width="44">
                             &nbsp;</td>
                            <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/white-ball.gif"></td>
                            <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/black-ball.gif"></td>
                            <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/black-ball.gif"></td>
                            <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/white-ball.gif"></td>       
                        </tr>
                    </tbody>
                    </table>
                </td>
            </tr>
        
            <tr>
                <td>
                    <b>White moves ahead:</b>
                 </td>
                 <td>
                    <table border="1" cellspacing="0" cellpadding="6" align="center">
                    <tbody>
                        <tr bgcolor="silver">
                            <td width="44">&nbsp;</td>
                            <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/white-ball.gif"></td>
                            <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/black-ball.gif"></td>
                            <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/black-ball.gif"></td>
                            <td width="44"><img src="https://akarsh-gupta007.github.io/backtracking/white-ball.gif"></td>       
                        </tr>
                    </tbody>
                    </table>
                </td>
            </tr>
        
            <tr>
                <td><b>stuck!</b></td>
                <td> &nbsp;</td>
            </tr>
            </tbody>
        </table>
        <p>
            Now to the program. The main program will initialize the board, and call a recursive backtracking routine to attempt to solve the puzzle. The backtracking routine will either succeed and print out a winning path, or it will fail, and the main program will have to print out the bad news.
        </p>
        <p>
            The backtracking method is named <font color="brown" size="4">solvable</font> and returns a <font color="brown" size="4">boolean</font>. In<font color="brown" size="4">solvable </font> we shall need to check whether we are at a leaf, which in this case means a position from which no further moves are possible. This isn't so easy.
        </p>
        <p>
            Each possible move will result in a new board position, and these new board positions are the children of the current board position. Hence to find the children of a node (that is, of a board position), we need only find the possible moves from that node. Remember that it is also highly desirable to find an ordering on these possible moves
        </p>
        <p>
            Here it is time to stop and take thought. To make progress, we must analyze the game to some extent. Probably a number of approaches would work, and what follows is based on the way I worked it out. If you were to program this puzzle, you might find a different but equally valid approach.
        </p>
        <p>
            First, notice that if a marble has a move, that move is unique: if it can move ahead one square, then it cannot jump. If it can jump, it cannot move ahead one square. This suggests that, to find the possible moves, we might assign numbers to the marbles, and check each marble in turn. When we have looked at all the marbles, we have looked at all the possible moves. This would require having a table to keep track of where each marble is, or else somehow "marking" each marble with its number and searching the board each time to find the marble we want. Neither alternative is very attractive.
        </p>
        <p>
            Next, notice that for a given board position, each marble occupies a unique space. Hence, instead of talking about moving a particular marble, we can talk about moving the marble in a particular space. If a move is possible from a given space, then that must be the only move possible from that space, because if the marble in that space has a move, it is unique. There is a slight complication because not every space contains a marble, but at least the spaces (unlike the marbles) stay in one place.
        </p>
        <p>
            Now we have a simpler ordering of moves to use in our program. Just check, in order, the<font color="brown" size="4"> 2n+1</font> spaces of the board. For each space, either zero or one moves is possible. With this understanding, we can write a boolean method <font color="brown" size="4">canMove(int[] board, int position)</font>
        </p>
        <ul>
            <li>If the position is empty, no move is possible</li>
            <li>If the position contains a black marble, the method checks for a move or jump to the right;</li>
            <li>If the position contains a white marble, the method checks for a move or jump to the left.</li>
        </ul>
        <p>
            We write another method <font color="brown" size="4">int[] makeMove(int[] oldBoard, int position)</font> that will take a board and a position, make a move from that position, and return as its value a new board. (We could write this somewhat more efficiently by changing the old board, rather than creating a new one, but here we are more concerned with simplicity.) In technical jargon, <font color="brown" size="4">makeMove </font>is "applicative" rather than "mutative."
        </p>
        <pre style="color: brown;font-size:16px">            boolean solvable(int[] board) {
                if (puzzleSolved(board)) {
                    return true;
                }
                for (int position = 0; position &lt; BOARD_SIZE; position++) {
                    if (canMove(board, position)) {
                        int[] newBoard = makeMove(board, position);
                        if (solvable(newBoard)) {
                            printBoard(newBoard);
                            return true;
                        }
                    }
                }
                return false;
            }
        </pre>
        <p>
            Along with <font color="brown" size="4">canMove</font> and<font color="brown" size="4">makeMove</font> , we are using methods<font color="brown" size="4"> puzzleSolved</font> And <font color="brown" size="4">printBoard</font> with meanings that should be obvious.
        </p>
        <p>
            Here is some output from the program:
        </p>
<pre align="left" style="font-size: 20px; "><span><i><font color="#0000ff">16.</font>  </i><b><font>WHITE  WHITE  WHITE  _____  BLACK  BLACK  BLACK</font></b></span>
<span><i><font color="#0000ff">15.</font></i>  <b><font>WHITE  WHITE  WHITE  BLACK  _____  BLACK  BLACK</font></b></span>
<span></span><i><font color="#0000ff">14.</font></i>  <b><font>WHITE  WHITE  _____  BLACK  WHITE  BLACK  BLACK</font></b>
<span></span><i><font color="#0000ff">13.</font></i>  <b><font>WHITE  _____  WHITE  BLACK  WHITE  BLACK  BLACK</font></b>
<span></span><i><font color="#0000ff">12.</font></i>  <b><font>WHITE  BLACK  WHITE  _____  WHITE  BLACK  BLACK</font></b>
<span></span><i><font color="#0000ff">11. </font></i> <b><font>WHITE  BLACK  WHITE  BLACK  WHITE  _____  BLACK</font></b>
<span></span><i><font color="#0000ff">10.</font></i> <b><font>WHITE  BLACK  WHITE  BLACK  WHITE  BLACK  _____</font></b>
<span></span><i><font color="#0000ff"> 9. </font></i> <b><font>WHITE  BLACK  WHITE  BLACK  _____  BLACK  WHITE</font></b>
<span></span><i><font color="#0000ff"> 8. </font></i> <b><font>WHITE  BLACK  _____  BLACK  WHITE  BLACK  WHITE</font></b>
<span></span><i><font color="#0000ff"> 7. </font></i> <b><font>_____  BLACK  WHITE  BLACK  WHITE  BLACK  WHITE</font></b>
<span></span><i><font color="#0000ff"> 6. </font></i> <b><font>BLACK  _____  WHITE  BLACK  WHITE  BLACK  WHITE</font></b>
<span></span><i><font color="#0000ff"> 5. </font></i> <b><font>BLACK  BLACK  WHITE  _____  WHITE  BLACK  WHITE</font></b>
<span></span><i><font color="#0000ff"> 4. </font></i> <b><font>BLACK  BLACK  WHITE  BLACK  WHITE  _____  WHITE</font></b>
<span></span><i><font color="#0000ff"> 3. </font></i> <b><font>BLACK  BLACK  WHITE  BLACK  _____  WHITE  WHITE</font></b>
<i><font color="#0000ff"> 2.</font></i>  <b><font>BLACK  BLACK  _____  BLACK  WHITE  WHITE  WHITE</font></b>
<i><font color="#0000ff"> 1. </font></i> <b><font>BLACK  BLACK  BLACK  _____  WHITE  WHITE  WHITE</font></b>

</pre>
<p>
    Notice that the solution is given in reverse order: <font size="3">BLACK</font> starts out on the left and<font size="3"> WHITE </font>on the right, as in the last line. I've added line numbers to the actual output in order to emphasize this point. Backtracking always produces its results (sequence of choices) in reverse order; it is up to you, the programmer, to reverse the results again to get them in the correct order.
</p>

</body></html>

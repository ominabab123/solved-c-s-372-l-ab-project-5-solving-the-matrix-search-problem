Download Link: https://assignmentchef.com/product/solved-c-s-372-l-ab-project-5-solving-the-matrix-search-problem
<br>









In this lab, you will design and implement algorithms to solve the matrix search problem—finding the maxima of each row in the matrix. The problem is disguised as different problems in many applications. They include breaking paragraphs into lines, sequence alignment, RNA secondary structure prediction, and image thresholding.

<h1>1        The matrix search problem</h1>

The matrix search problem finds the maxima of each row in a matrix.

Input: an <em>m </em>× <em>n </em>matrix

Output: the maximum element of each row, marked in red.

<h1>2        A brute­force iterative solution</h1>

This algorithm searches for the maxima row by row. On each row, it linearly scans each element to find the maximum. The runtime is apparently <em>O</em>(<em>mn</em>). Implement this algorithm using a C++ program. Please define your function as follows:

template&lt;typename T&gt;

vector&lt;T&gt; find_row_maxima_itr(const matrix&lt;T&gt; &amp; m)

{

// Your code here

}

//[[Rcpp::export]] vector&lt;double&gt; row_maxima_itr

(const vector&lt;double&gt; &amp; v, size_t nrow, size_t ncol)

{ matrix&lt;double&gt; mat(v, nrow, ncol); return find_row_maxima_itr(mat);

}

The input matrix is represented in the vector v column by column (not row by row). This choice is made so that the R matrix class can be passed to the C++ vector class without any data type conversion, saving time. Internally in R, a matrix is already stored as a column­major vector.

This function uses a C++ template matrix class given in the skeleton code file MatrixSearch-Skeleton.cpp.

<h1>3        Divide­and­conquer monotonic matrices</h1>

Let <em>j</em>(<em>i</em>) define the smallest column index to the maximum element in row <em>i</em>. A matrix is monotone if for any <em>i</em><sub>1 </sub><em>&lt; i</em><sub>2</sub>, <em>j</em>(<em>i</em><sub>1</sub>) ≤ <em>j</em>(<em>i</em><sub>2</sub>). In the following example

we have the following column positions of the row maxima <em>j</em>(0) = 1 ≤ <em>j</em>(1) = 1 ≤ <em>j</em>(2) = 2 ≤ <em>j</em>(3) = 4 ≤ <em>j</em>(4) = 4 ≤ <em>j</em>(5) = 4

Please note that the column indices of the row maxima are non­decreasing (1, 1, 2, 4, 4, 4), but the row maximum values are not necessarily increasing. In this example, they are 4, 8, ­1, 2.5, 3, 10—the row maximum values can go up and down.

For a monotonic matrix, we can perform divide­and­conquer on the rows to solve the matrix search problem much faster than the brute­force iterative solution. Please design a divide­and­conquer strategy to achieve a runtime of <em>O</em>(<em>n </em>lg <em>m</em>), using the following template

template&lt;typename T&gt;

vector&lt;T&gt; find_row_maxima(const matrix&lt;T&gt; &amp; m)

{

// Your divide-and-conquer code here

}

//[[Rcpp::export]] vector&lt;double&gt; row_maxima

(const vector&lt;double&gt; &amp; v, size_t nrow, size_t ncol)

{ matrix&lt;double&gt; mat(v, nrow, ncol); return find_row_maxima(mat);

}

<h1>4        Test the two functions</h1>

Develop a C/C++ test function to include five examples to check your functions. If a function passes the tests, minimal output should be displayed on the screen to indicate success; otherwise, point out which example the function failed. This test function should take a function parameter so that it can test both of your functions, defined as follows:

bool test_row_maxima

(vector&lt;double&gt; (*rmfun) (const vector&lt;double&gt; &amp; v, size_t nrow, size_t ncol))

{

// Example 1 bool passed = true;

/* Monotonic matrix example 1:

0, 4, -1, 2.5, -4,

-3, 8, -10, 2, 7,

-4, -3, -1, -100, -5.5,

0, 2, 0.3, -3, 2.5,

1, 0, 1, 2, 3,

-8, 9, 2, 5, 10};

*/

// x is column major vectorization of the matrix double x[] = { 0, -3, -4,  0, 1, -8, 4,    8,   -3,  2, 0, 9,

-1, -10,    -1, 0.3, 1, 2,

2.5,    2, -100, -3, 2, 5,

-4,   7, -5.5, 2.5, 3, 10};

vector&lt;double&gt; v(x, x+30); double rmax_truth[] = {4, 8, -1, 2.5, 3, 10};

if(rmfun(v, 6, 5) != vector&lt;double&gt;(rmax_truth, rmax_truth+6)) {

cout &lt;&lt; ”ERROR: failed test 1!” &lt;&lt; endl; passed = false;

}

// Example 2 …

// Example 5 return passed;

}

//[[Rcpp::export]] bool testall()

{ bool passed = true; if(!test_row_maxima(row_maxima_itr)) {

cout &lt;&lt; ”ERROR: row_maxima_itr() failed some test!” &lt;&lt; endl; passed = false;

} if(!test_row_maxima(row_maxima)) { cout &lt;&lt; ”ERROR: row_maxima() failed some test!” &lt;&lt; endl; passed = false;

}

if(passed) {

cout &lt;&lt; ”All tests passed. Congratulations!” &lt;&lt; endl;

}

return passed;

}

Your C++ program must include a main() function that calls the testall() function. When the program is compiled by a C++ compiler it generates a binary executable file that will run when invoked from the command line.

<h1>5        Visualize the runtime of the two methods</h1>

Develop R code inside your C++ source code file to generate runtime report on the two functions you developed.

You should use the following R function to generate any sized monotonic matrix: random.monotone.matrix(nrow, ncol)

which is already provided in the skeleton code. In the runtime evaluation you can focus on testing square matrices (<em>m </em>= <em>n</em>).

You will generate one plot containing four curves as shown in Figure 1. It shows both empirical runtime and estimated theoretical runtime. Your code will estimate the coefficients <em>c</em><sub>1 </sub>and <em>c</em><sub>2 </sub>so that you can plot the theoretical curves fitted to the runtime data.

You need to call the testall() function first. If the test failed, visualization should not proceed and you must fix the code first.



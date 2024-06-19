LaTeX 수식 문법에 대한 공식 문서를 참조하여 전체 EBNF 정의를 개선했습니다. 주요 참고 문서로는 AMS-LaTeX 가이드, CTAN의 amsmath 패키지 문서, 그리고 Short Math Guide for LaTeX가 사용되었습니다.

### LaTeX 수식 문법 EBNF 개선된 버전

```ebnf
// Main expression
expression    ::= align_env | equation_env | sum_expr | prod_expr | int_expr | lim_expr | basic_expr

// Basic expressions including addition and subtraction
basic_expr    ::= term (("+" | "-" | "\\pm" | "\\mp" | "\\cup" | "\\cap" | "\\setminus") term)*

// Terms including multiplication and division
term          ::= factor (("*" | "/" | "\\cdot" | "\\times" | "\\div" | "\\land" | "\\lor" | "\\wedge" | "\\vee") factor)*

// Factors including exponentiation and subscripts
factor        ::= base ("^" exponent)? ("_" subscript)?
base          ::= number | variable | function | fraction | sqrt | nroot | matrix | binom | logical_op | "{" expression "}" | "(" expression ")" | overline | underline | text | accent | operator | delimiter | font_style | color
exponent      ::= factor
subscript     ::= factor

// Numbers and variables
number        ::= digit+ ("." digit+)?
variable      ::= letter+ | greek_letter

// Functions including trigonometric and logarithmic functions
function      ::= function_name "(" expression ")" | function_name "{" expression "}"
function_name ::= "sin" | "cos" | "tan" | "cot" | "sec" | "csc" | "log" | "ln" | "exp" | "sqrt" | "max" | "min" | "sum" | "prod" | "lim" | "int" | "frac" | "binom" | "arcsin" | "arccos" | "arctan" | "sinh" | "cosh" | "tanh"

// Fractions
fraction      ::= "\\frac" "{" expression "}" "{" expression "}"

// Square roots and nth roots
sqrt          ::= "\\sqrt" "{" expression "}"
nroot         ::= "\\sqrt" "[" expression "]" "{" expression "}"

// Binomial coefficients
binom         ::= "\\binom" "{" expression "}" "{" expression "}"

// Matrices
matrix        ::= "\\begin{matrix}" matrix_rows "\\end{matrix}" |
                  "\\begin{pmatrix}" matrix_rows "\\end{pmatrix}" |
                  "\\begin{bmatrix}" matrix_rows "\\end{bmatrix}" |
                  "\\begin{Bmatrix}" matrix_rows "\\end{Bmatrix}" |
                  "\\begin{vmatrix}" matrix_rows "\\end{vmatrix}" |
                  "\\begin{Vmatrix}" matrix_rows "\\end{Vmatrix}"
matrix_rows   ::= matrix_row ("\\\\" matrix_row)*
matrix_row    ::= expression ("&" expression)*

// Align and equation environments
align_env     ::= "\\begin{align}" align_rows "\\end{align}" |
                  "\\begin{align*}" align_rows "\\end{align*}"
align_rows    ::= align_row ("\\\\" align_row)*
align_row     ::= expression ("&" expression)*

equation_env  ::= "\\begin{equation}" expression "\\end{equation}" |
                  "\\begin{equation*}" expression "\\end{equation*}"

// Sum, product, and integral with limits
sum_expr      ::= "\\sum" "_" subscript "^" exponent "{" expression "}"
prod_expr     ::= "\\prod" "_" subscript "^" exponent "{" expression "}"
int_expr      ::= "\\int" "_" lower_limit "^" upper_limit "{" expression "}"
lim_expr      ::= "\\lim" "_" subscript "{" expression "}"

lower_limit   ::= expression
upper_limit   ::= expression

// Overline and underline
overline      ::= "\\overline" "{" expression "}"
underline     ::= "\\underline" "{" expression "}"

// Text
text          ::= "\\text" "{" text_content "}"
text_content  ::= (letter | digit | " " | symbol)*

// Accents
accent        ::= "\\hat" "{" expression "}" |
                  "\\bar" "{" expression "}" |
                  "\\vec" "{" expression "}" |
                  "\\dot" "{" expression "}" |
                  "\\ddot" "{" expression "}" |
                  "\\tilde" "{" expression "}" |
                  "\\breve" "{" expression "}" |
                  "\\check" "{" expression "}" |
                  "\\acute" "{" expression "}" |
                  "\\grave" "{" expression "}"

// Operators
operator      ::= "+" | "-" | "*" | "/" | "^" | "_" | "=" | "<" | ">" |
                  "\\leq" | "\\geq" | "\\neq" | "\\approx" | "\\equiv" | 
                  "\\sim" | "\\simeq" | "\\cong" | "\\propto" | 
                  "\\infty" | "\\partial" | "\\nabla" | "\\forall" |
                  "\\exists" | "\\neg" | "\\land" | "\\lor" | "\\to" |
                  "\\implies" | "\\iff" | "\\int" | "\\sum" | "\\prod" | 
                  "\\cup" | "\\cap" | "\\setminus"

// Delimiters
delimiter     ::= "(" | ")" | "[" | "]" | "{" | "}" | "|" | "\\|" | "\\langle" | "\\rangle"

// Character sets
digit         ::= "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"
letter        ::= "a" | "b" | "c" | "d" | "e" | "f" | "g" | "h" | "i" | "j" | "k" 
                | "l" | "m" | "n" | "o" | "p" | "q" | "r" | "s" | "t" | "u" | "v" 
                | "w" | "x" | "y" | "z" | "A" | "B" | "C" | "D" | "E" | "F" | "G" 
                | "H" | "I" | "J" | "K" | "L" | "M" | "N" | "O" | "P" | "Q" | "R" 
                | "S" | "T" | "U" | "V" | "W" | "X" | "Y" | "Z"
greek_letter  ::= "alpha" | "beta" | "gamma" | "delta" | "epsilon" | "zeta" | "eta" |
                  "theta" | "iota" | "kappa" | "lambda" | "mu" | "nu" | "xi" |
                  "omicron" | "pi" | "rho" | "sigma" | "tau" | "upsilon" | "phi" |
                  "chi" | "psi" | "omega" | "Alpha" | "Beta" | "Gamma" | "Delta" |
                  "Epsilon" | "Zeta" | "Eta" | "Theta" | "Iota" | "Kappa" | "Lambda" |
                  "Mu" | "Nu" | "Xi" | "Omicron" | "Pi" | "Rho" | "Sigma" | "Tau" |
                  "Upsilon" | "Phi" | "Chi" | "Psi" | "Omega"

// Logical operators
logical_op    ::= "\\land" | "\\lor" | "\\neg" | "\\implies" | "\\iff" |
                  "\\forall" | "\\exists" | "\\in" | "\\notin" | "\\subset" | 
                  "\\supset" | "\\subseteq" | "\\supseteq" | "\\setminus" | "\\emptyset"

// Symbols for text content
symbol        ::= "!" | "\"" | "#" | "$" | "%" | "&" | "'" | "(" | ")" | "*" | "+" |
                  "," | "-" | "." | "/" | ":" | ";" | "<" | "=" | ">" | "?" | "@" |
                  "[" | "\\" | "]" | "^" | "_" | "`" | "{" | "|" | "}" | "~"

// Font styles
font_style    ::= "\\mathrm{" text_content "}" |
                  "\\mathit{" text_content "}" |
                  "\\mathbf{" text_content "}" |
                  "\\mathsf{" text_content "}" |
                  "\\mathtt{" text_content "}" |
                  "\\mathcal{" text_content "}" |
                  "\\mathbb{" text_content "}"

// Font size
font_size     ::= "\\tiny" | "\\scriptsize" | "\\footnotesize" | "\\small" |
                  "\\normalsize" | "\\large" | "\\Large" | "\\LARGE" |
                  "\\huge" | "\\Huge"

// Colors (requires xcolor package)
color         ::= "\\color{" color_name "}" |
                  "\\textcolor{" color_name "}{" text_content "}" |
                  "\\colorbox{" color_name "}{" text_content "}"
color_name    ::= "red" | "green" | "blue" | "cyan" | "magenta" | "yellow" | "black" | "white" | "gray" | "brown" | "lime" | "olive" | "orange" | "pink" | "purple" | "teal" | "violet"

// Size adjustment
size_adjustment ::= "\\scalebox{" factor "}{" expression "}" |
                    "\\resizebox{" width "}{" height "}{" expression "}" |
                    "\\rotatebox{" angle "}{" expression "}"
factor        ::= number
width         ::= number unit
height        ::= number unit
angle         ::= number
unit          ::= "pt" | "mm" | "cm" | "in" | "ex" | "em" | "bp" | "dd" | "pc"

// Additional mathematical symbols
additional_math_symbols ::= "\\aleph" | "\\beth" | "\\gimel" | "\\daleth" |
                            "\\hbar" | "\\imath" | "\\jmath" | "\\ell" | "\\wp" | "\\Re" | "\\Im" |
                            "\\aleph" | "\\mho" | "\\prime" | "\\emptyset" | "\\infty" | "\\nabla" | "\\surd" |
                            "\\top" | "\\bot" | "\\angle" | "\\measuredangle" | "\\sphericalangle" |
                            "\\backslash" | "\\bigtriangleup" | "\\bigtriangledown" | "\\triangleleft" | "\\triangleright" |
                            "\\lozenge" | "\\bigstar" | "\\blacklozenge" | "\\blacksquare" | "\\blacktriangle" | "\\blacktriangledown" | "\\blacktriangleleft" | "\\blacktriangleright"

// Environments for arrays and tabular
array_env    ::= "\\begin{array}{" alignment "}" array_rows "\\end{array}"
alignment    ::= "l" | "c" | "r"
array_rows   ::= array_row ("\\\\" array_row)*
array_row    ::= expression ("&" expression)*

tabular_env  ::= "\\begin{tabular}{" alignment "}" tabular_rows "\\end{tabular}"
tabular_rows ::= tabular_row ("\\\\" tabular_row)*
tabular_row  ::= expression ("&" expression)*
```

이 문법 정의는 AMS-LaTeX 가이드 및 CTAN의 amsmath 패키지 문서에서 제공하는 정보를 바탕으로 하여, LaTeX 수식의 다양한 구성 요소를 포괄적으로 다루도록 개선되었습니다.

참고 문서:
- [User's Guide for the amsmath Package](https://www.ams.org/publications/authors/tex/amslatex)
- [Short Math Guide for LaTeX](https://tug.ctan.org/info/short-math-guide/short-math-guide.pdf)
- [AMS-LaTeX on CTAN](https://ctan.org/pkg/amsmath)

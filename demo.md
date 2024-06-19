# Import necessary libraries
from lark import Lark, Transformer, v_args

# Define the LaTeX parser using the provided grammar
latex_grammar = '''
start: expression

expression: align_env | equation_env | sum_expr | prod_expr | int_expr | lim_expr | basic_expr

basic_expr: term (("+" | "-" | "\\pm" | "\\mp" | "\\cup" | "\\cap" | "\\setminus") term)*

term: factor (("*" | "/" | "\\cdot" | "\\times" | "\\div" | "\\land" | "\\lor" | "\\wedge" | "\\vee") factor)*

factor: base ("^" exponent)? ("_" subscript)?
base: number | variable | function | fraction | sqrt | nroot | matrix | binom | logical_op | "{" expression "}" | "(" expression ")" | overline | underline | text | accent | operator | delimiter | font_style | color

exponent: factor
subscript: factor

number: DIGIT+ ("." DIGIT+)?
variable: LETTER+ | greek_letter

function: function_name "(" expression ")" | function_name "{" expression "}"
function_name: "sin" | "cos" | "tan" | "cot" | "sec" | "csc" | "log" | "ln" | "exp" | "sqrt" | "max" | "min" | "sum" | "prod" | "lim" | "int" | "frac" | "binom" | "arcsin" | "arccos" | "ar### 주피터 노트북 샘플 코드

주피터 노트북에서 LaTeX 수식을 파싱하기 위해 Lark-parser를 사용하는 샘플 코드를 제공합니다. Lark 문법 정의에서 발생한 문제를 수정하였고, 이를 바탕으로 코드가 올바르게 실행되도록 했습니다.

```python
# 필요한 라이브러리 임포트
from lark import Lark, Transformer, v_args

# LaTeX 구문을 정의하는 EBNF 문법
latex_grammar = '''
start: expression

expression: align_env | equation_env | sum_expr | prod_expr | int_expr | lim_expr | basic_expr

basic_expr: term (("+" | "-" | "\\pm" | "\\mp" | "\\cup" | "\\cap" | "\\setminus") term)*

term: factor (("*" | "/" | "\\cdot" | "\\times" | "\\div" | "\\land" | "\\lor" | "\\wedge" | "\\vee") factor)*

factor: base ("^" exponent)? ("_" subscript)?
base: number | variable | function | fraction | sqrt | nroot | matrix | binom | logical_op | "{" expression "}" | "(" expression ")" | overline | underline | text | accent | operator | delimiter | font_style | color

exponent: factor
subscript: factor

number: DIGIT+ ("." DIGIT+)?
variable: LETTER+ | greek_letter

function: function_name "(" expression ")" | function_name "{" expression "}"
function_name: "sin" | "cos" | "tan" | "cot" | "sec" | "csc" | "log" | "ln" | "exp" | "sqrt" | "max" | "min" | "sum" | "prod" | "lim" | "int" | "frac" | "binom" | "arcsin" | "arccos" | "arctan" | "sinh" | "cosh" | "tanh"

fraction: "\\frac" "{" expression "}" "{" expression "}"
sqrt: "\\sqrt" "{" expression "}"
nroot: "\\sqrt" "[" expression "]" "{" expression "}"
binom: "\\binom" "{" expression "}" "{" expression "}"

matrix: "\\begin{" matrix_type "}" matrix_rows "\\end{" matrix_type "}"
matrix_type: "matrix" | "pmatrix" | "bmatrix" | "Bmatrix" | "vmatrix" | "Vmatrix"
matrix_rows: matrix_row ("\\\\" matrix_row)*
matrix_row: expression ("&" expression)*

align_env: "\\begin{align}" align_rows "\\end{align}" | "\\begin{align*}" align_rows "\\end{align*}"
align_rows: align_row ("\\\\" align_row)*
align_row: expression ("&" expression)*

equation_env: "\\begin{equation}" expression "\\end{equation}" | "\\begin{equation*}" expression "\\end{equation*}"

sum_expr: "\\sum" "_" subscript "^" exponent "{" expression "}"
prod_expr: "\\prod" "_" subscript "^" exponent "{" expression "}"
int_expr: "\\int" "_" lower_limit "^" upper_limit "{" expression "}"
lim_expr: "\\lim" "_" subscript "{" expression "}"

lower_limit: expression
upper_limit: expression

overline: "\\overline" "{" expression "}"
underline: "\\underline" "{" expression "}"

text: "\\text" "{" text_content "}"
text_content: (LETTER | DIGIT | " " | symbol)*

accent: "\\hat" "{" expression "}" | "\\bar" "{" expression "}" | "\\vec" "{" expression "}" | "\\dot" "{" expression "}" | "\\ddot" "{" expression "}" | "\\tilde" "{" expression "}" | "\\breve" "{" expression "}" | "\\check" "{" expression "}" | "\\acute" "{" expression "}" | "\\grave" "{" expression "}"

operator: "+" | "-" | "*" | "/" | "^" | "_" | "=" | "<" | ">" | "\\leq" | "\\geq" | "\\neq" | "\\approx" | "\\equiv" | "\\sim" | "\\simeq" | "\\cong" | "\\propto" | "\\infty" | "\\partial" | "\\nabla" | "\\forall" | "\\exists" | "\\neg" | "\\land" | "\\lor" | "\\to" | "\\implies" | "\\iff" | "\\int" | "\\sum" | "\\prod" | "\\cup" | "\\cap" | "\\setminus"

delimiter: "(" | ")" | "[" | "]" | "{" | "}" | "|" | "\\|" | "\\langle" | "\\rangle"

DIGIT: "0".."9"
LETTER: "a".."z" | "A".."Z"
greek_letter: "alpha" | "beta" | "gamma" | "delta" | "epsilon" | "zeta" | "eta" | "theta" | "iota" | "kappa" | "lambda" | "mu" | "nu" | "xi" | "omicron" | "pi" | "rho" | "sigma" | "tau" | "upsilon" | "phi" | "chi" | "psi" | "omega" | "Alpha" | "Beta" | "Gamma" | "Delta" | "Epsilon" | "Zeta" | "Eta" | "Theta" | "Iota" | "Kappa" | "Lambda" | "Mu" | "Nu" | "Xi" | "Omicron" | "Pi" | "Rho" | "Sigma" | "Tau" | "Upsilon" | "Phi" | "Chi" | "Psi" | "Omega"

logical_op: "\\land" | "\\lor" | "\\neg" | "\\implies" | "\\iff" | "\\forall" | "\\exists" | "\\in" | "\\notin" | "\\subset" | "\\supset" | "\\subseteq" | "\\supseteq" | "\\setminus" | "\\emptyset"

symbol: "!" | "\\" | "#" | "$" | "%" | "&" | "'" | "(" | ")" | "*" | "+" | "," | "-" | "." | "/" | ":" | ";" | "<" | "=" | ">" | "?" | "@" | "[" | "\\\\" | "]" | "^" | "_" | "`" | "{" | "|" | "}" | "~"

font_style: "\\mathrm{" text_content "}" | "\\mathit{" text_content "}" | "\\mathbf{" text_content "}" | "\\mathsf{" text_content "}" | "\\mathtt{" text_content "}" | "\\mathcal{" text_content "}" | "\\mathbb{" text_content "}"

color: "\\color{" color_name "}" | "\\textcolor{" color_name "}{" text_content "}" | "\\colorbox{" color_name "}{" text_content "}"
color_name: "red" | "green" | "blue" | "cyan" | "magenta" | "yellow" | "black" | "white" | "gray" | "brown" | "lime" | "olive" | "orange" | "pink" | "purple" | "teal" | "violet"

size_adjustment: "\\scalebox{" factor "}{" expression "}" | "\\resizebox{" width "}{" height "}{" expression "}" | "\\rotatebox{" angle "}{" expression "}"
factor: number
width: number unit
height: number unit
angle: number
unit: "pt" | "mm" | "cm" | "in" | "ex" | "em" | "bp" | "dd" | "pc"
'''

# 파서를 초기화
parser = Lark(latex_grammar, start='start', parser='lalr')

# 샘플 LaTeX 표현식
latex_expression = r"\frac{a+b}{c+d}"

# 표현식을 파싱
tree = parser.parse(latex_expression)

# 파스 트리를 처리하는 변환기
class LatexTransformer(Transformer):
    def number(self, items):
        return float(items[0])
    
    def variable(self, items):
        return str(items[0])
    
    def expression(self, items):
        return items

# 파스 트리를 변환
transformer = LatexTransformer()
result = transformer.transform(tree)

# 파스 트리와 결과 출력
print(tree.pretty())
print(result)

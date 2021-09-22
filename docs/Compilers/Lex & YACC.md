# Lex & YACC

Ref: [Lex与YACC详解](https://zhuanlan.zhihu.com/p/143867739)

## Lex

Lex is a computer program that **generates lexical analyzers**.

e.g.1

```c
%{
#include <stdio.h>
%}

%%
[0123456789]+ printf("NUMBER\n");
[a-zA-Z][a-zA-Z0-9]* printf("WORD\n");
```

```shell
lex eg.l
cc lex.yy.c –o test –ll
./test
```

e.g.2

```c
%{
#include <stdio.h>
%}

%%
[a-zA-Z][a-zA-Z0-9]* printf("WORD ");
[a-zA-Z0-9\/.-]+ printf("FILENAME ");
\" printf("QUOTE ");
\{ printf("OBRACE ");
\} printf("EBRACE ");
; printf("SEMICOLON ");
\n printf("\n");
[ \t]+ /* ignore whitespace */;
%%
```

File to parse:

```c
logging{
category lame-servers { null; };
category cname { null; };
};

zone "." {
type hint;
file "/etc/bind/db.root";
}
```

## YACC

Yet Another Compiler-Compiler is a [Look Ahead Left-to-Right (LALR) parser generator](https://en.wikipedia.org/wiki/LALR_parser_generator), generating a [LALR parser](https://en.wikipedia.org/wiki/LALR_parser) (the part of a compiler that tries to make syntactic sense of the source code) based on a formal grammar.

```c
// temp.l
%{
#include <stdio.h>
#include "y.tab.h"
%}

%%
[0-9]+ yylval = atoi(yytext); return NUMBER;
heat return TOKHEAT;
on|off yylval = !strcmp(yytext, "on"); return STATE;
target return TOKTARGET;
temperature return TOKTEMPERATURE;
\n /* ignore end of line */
[ \t]+ /* ignore whitespace */
%%
```

```c
// temp.y
%{
#include <stdio.h>
#include <string.h>

//在lex.yy.c里定义，会被yyparse()调用。在此声明消除编译和链接错误。
extern int yylex(void); 

// 在此声明，消除yacc生成代码时的告警
extern int yyparse(void); 

int yywrap()
{
	return 1;
}

// 该函数在y.tab.c里会被调用，需要在此定义
void yyerror(const char *s)
{
	printf("[error] %s\n", s);
}

int main()
{
	yyparse();
	return 0;
}
%}

%token NUMBER TOKHEAT STATE TOKTARGET TOKTEMPERATURE

%%
commands: /* empty */
| commands command
;

command: heat_switch | target_set ;

heat_switch:
TOKHEAT STATE
{
    if ($2)
        printf("\tHeat turned on\n");
    else
        printf("\tHeat turned off\n");
}

target_set:
TOKTARGET TOKTEMPERATURE NUMBER
{
    printf("\tTemperature set to %d\n", $3);
};
%%
```

Commands:

```shell
❯ ./a.out
heat on
	Heat turned on
target temperature 10
	Temperature set to 10
```


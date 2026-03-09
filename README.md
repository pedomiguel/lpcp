# lpcp

An interpreted, typed programming language written in Haskell.

## Project structure

```
app/
  Main.hs                   Entry point. Starts the REPL or runs a source file.

src/Language/LPCP/
  Token.hs                  Defines the Token type and TokenPos (source position).
                            Every symbol the lexer can produce lives here.

  Lexer.hs                  Converts a raw source string into a flat list of tokens.
                            This is the first stage of the pipeline.

  AST.hs                    Defines the Abstract Syntax Tree: Expr, Stmt, Type,
                            BinOp, UnOp. This is the shared data structure that
                            the parser produces and all later stages consume.

  Parser.hs                 Converts the token stream into an AST.
                            Handles operator precedence and syntax rules.

  Env.hs                    A generic variable environment (name -> value map).
                            Used by both the type checker and the evaluator
                            to track bindings in scope.

  TypeChecker.hs            Traverses the AST and infers/checks types.
                            Rejects ill-typed programs before evaluation.

  Evaluator.hs              Traverses the type-checked AST and produces runtime
                            values. Implements the actual semantics of the language.

  REPL.hs                   Glues the pipeline together (lex -> parse -> typecheck
                            -> eval) and provides the interactive REPL loop and
                            file runner.

test/
  LexerSpec.hs              Tests for the lexer.
  ParserSpec.hs             Tests for the parser.
  TypeCheckerSpec.hs        Tests for the type checker.
  EvaluatorSpec.hs          Tests for the evaluator.

examples/
  hello.lpcp                Example source file showing basic language features.
```

## Pipeline

```
source code
    |
  Lexer        source string -> [(Token, TokenPos)]
    |
  Parser       token stream  -> Program (AST)
    |
  TypeChecker  AST           -> TypeEnv (or TypeError)
    |
  Evaluator    AST           -> [(Name, Value)]
```

## Building and running

```sh
cabal build
cabal test
cabal run lpcp                        # start the REPL
cabal run lpcp -- examples/hello.lpcp # run a file
```

# Testing Minishell Tokenizer

## Basic Quote Tests

```bash
echo hello world
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: 'hello', Type: WORD
# Token: 'world', Type: WORD

echo "hello world"
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: 'hello world', Type: WORD (double-quoted)

echo 'hello world'
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: 'hello world', Type: WORD (single-quoted)
```

## Adjacent Quotes Tests

```bash
echo "hello"world
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: 'helloworld', Type: WORD (contains double quotes)

echo hello"world"
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: 'helloworld', Type: WORD (contains double quotes)

echo hel"lo"wor'ld'
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: 'helloworld', Type: WORD (contains both quote types)
```

## Empty Quote Tests

```bash
echo ""
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: '', Type: WORD (double-quoted)

echo ''
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: '', Type: WORD (single-quoted)

echo ""''
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: '', Type: WORD (contains both quote types)
```

## Multiple Adjacent Quotes

```bash
echo """"""
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: '', Type: WORD (multiple double quotes)

echo ''''''
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: '', Type: WORD (multiple single quotes)

echo "''"'""'
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: '\'\'""', Type: WORD (interleaved quotes)
```

## Quotes with Operators

```bash
echo "hello"|cat
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: 'hello', Type: WORD (double-quoted)
# Token: '|', Type: PIPE
# Token: 'cat', Type: WORD

echo hel"lo"|cat
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: 'hello', Type: WORD (contains double quotes)
# Token: '|', Type: PIPE
# Token: 'cat', Type: WORD

echo "hello>world"
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: 'hello>world', Type: WORD (quoted operator shouldn't be recognized as an operator)
```

## Quotes with Spaces

```bash
echo "  hello  "
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: '  hello  ', Type: WORD (spaces preserved in quotes)

echo "hello"  "world"
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: 'hello', Type: WORD (double-quoted)
# Token: 'world', Type: WORD (double-quoted)
```

## Unclosed Quotes

```bash
echo "hello
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: 'hello', Type: WORD (unclosed double quote)

echo 'hello
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: 'hello', Type: WORD (unclosed single quote)
```

## Mixed Quote Types

```bash
echo "This is 'quoted'"
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: 'This is \'quoted\'', Type: WORD (double-quoted)

echo 'This is "quoted"'
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: 'This is "quoted"', Type: WORD (single-quoted)
```

## Command Tests

```bash
ls -l | grep "file name" > "output.txt"
# Should tokenize as:
# Token: 'ls', Type: WORD
# Token: '-l', Type: WORD
# Token: '|', Type: PIPE
# Token: 'grep', Type: WORD
# Token: 'file name', Type: WORD (double-quoted)
# Token: '>', Type: REDIR_OUT
# Token: 'output.txt', Type: WORD (double-quoted)
```

## Quote Tests with Operators Without Spaces

```bash
echo "hello">file.txt
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: 'hello', Type: WORD (double-quoted)
# Token: '>', Type: REDIR_OUT
# Token: 'file.txt', Type: WORD

echo"hello"
# Should tokenize as:
# Token: 'echohello', Type: WORD (contains quotes)
```

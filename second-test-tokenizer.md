# More Minishell Tokenizer Test Cases

## Complex Quote Combinations

```bash
echo "'$USER'"
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: '\'$USER\'', Type: WORD (contains single AND double quotes)

echo "a'b'c'd'"
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: '"a\'b\'c\'d\'"', Type: WORD (double-quoted)

echo 'a"b"c"d"'
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: '\'a"b"c"d"\'', Type: WORD (single-quoted)
```

## Multiple Operators & Redirections

```bash
echo hello > file1 >> file2
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: 'hello', Type: WORD
# Token: '>', Type: REDIR_OUT
# Token: 'file1', Type: WORD
# Token: '>>', Type: APPEND
# Token: 'file2', Type: WORD

cat < file1 | grep pattern > file2
# Should tokenize as:
# Token: 'cat', Type: WORD
# Token: '<', Type: REDIR_IN
# Token: 'file1', Type: WORD
# Token: '|', Type: PIPE
# Token: 'grep', Type: WORD
# Token: 'pattern', Type: WORD
# Token: '>', Type: REDIR_OUT
# Token: 'file2', Type: WORD
```

## Heredoc Syntax

```bash
cat << EOF
# Should tokenize as:
# Token: 'cat', Type: WORD
# Token: '<<', Type: HEREDOC
# Token: 'EOF', Type: WORD

cat << "EOF"
# Should tokenize as:
# Token: 'cat', Type: WORD
# Token: '<<', Type: HEREDOC
# Token: '"EOF"', Type: WORD (double-quoted)

cat<<"EOF"
# Should tokenize as:
# Token: 'cat', Type: WORD
# Token: '<<', Type: HEREDOC
# Token: '"EOF"', Type: WORD (double-quoted)
```

## Operators Inside Words

```bash
echo a>b
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: 'a', Type: WORD
# Token: '>', Type: REDIR_OUT
# Token: 'b', Type: WORD

echo a">"b
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: 'a">"b', Type: WORD (double-quoted)
```

## Variable Usage Patterns

```bash
echo $USER
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: '$USER', Type: WORD

echo "$USER"
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: '"$USER"', Type: WORD (double-quoted)

echo '$USER'
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: '\'$USER\'', Type: WORD (single-quoted)

echo "$USER"hello
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: '"$USER"hello', Type: WORD (double-quoted)
```

## Spaces and Special Characters

```bash
echo "hello    world"
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: '"hello    world"', Type: WORD (double-quoted)

echo ?
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: '?', Type: WORD

echo $?
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: '$?', Type: WORD
```

## Edge Cases with Operators

```bash
echo |
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: '|', Type: PIPE

>
# Should tokenize as:
# Token: '>', Type: REDIR_OUT

echo > > file
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: '>', Type: REDIR_OUT
# Token: '>', Type: REDIR_OUT
# Token: 'file', Type: WORD

echo a""b''c
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: 'a""b\'\'c', Type: WORD (contains both quote types)
```

## Complex Command Lines

```bash
echo "one two" three 'four five' | grep "something" > "file name" 2>&1
# Should tokenize as:
# Token: 'echo', Type: WORD
# Token: '"one two"', Type: WORD (double-quoted)
# Token: 'three', Type: WORD
# Token: '\'four five\'', Type: WORD (single-quoted)
# Token: '|', Type: PIPE
# Token: 'grep', Type: WORD
# Token: '"something"', Type: WORD (double-quoted)
# Token: '>', Type: REDIR_OUT
# Token: '"file name"', Type: WORD (double-quoted)
# Token: '2>&1', Type: WORD

ls -la | grep "^d" | sort | head -5 > result.txt
# Should tokenize as:
# Token: 'ls', Type: WORD
# Token: '-la', Type: WORD
# Token: '|', Type: PIPE
# Token: 'grep', Type: WORD
# Token: '"^d"', Type: WORD (double-quoted)
# Token: '|', Type: PIPE
# Token: 'sort', Type: WORD
# Token: '|', Type: PIPE
# Token: 'head', Type: WORD
# Token: '-5', Type: WORD
# Token: '>', Type: REDIR_OUT
# Token: 'result.txt', Type: WORD
```

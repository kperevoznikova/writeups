# lost exponent

We have an encryption algorythm implemented as Python script.
In the third line the secret key `e` and variable `flag` are imported from unknown module `lost`.

First of all we should notice that we use a pseudorandom number generator with a known seed `6174` to shuffle a sequence.
Next, all of transformations of the given matrix are inversible:

```
if __name__ == '__main__':
    m = Matrix()
    for i, f in zip(order, flag): 
        m[i] = ord(f) # create a square matrix text length wide
    cflag = list(map(str, m ** e)) # every element of the matrix multiplies by secret key e 
    mn = max(map(len, cflag)) # get the biggest number
    mn += mn % 2 # add one if it's length is odd
    cflag = ''.join(b.zfill(mn) for b in cflag) # every line is filled to the resulting size of block 
    cflag = bytes([int(cflag[i:i+2]) for i in range(0, len(cflag), 2)])
 
    with open('enc', 'wb') as out:
        out.write(cflag)

```
The only thing we don't know is how to invert the shuffle transformation with a given seed.
I used [this crypto.stackexchange answer](https://crypto.stackexchange.com/questions/78309/how-to-get-the-original-list-back-given-a-shuffled-list-and-seed) to do it. After applying inverse transformations we check results using known first bytes of the flag given by this `assert flag.startswith('CTF-BR{')` line.

The flag is `CTF-BR{s0M3_0F_m47r1X_106}`.
# Lua Virtual Machine


A Lua virtual machine for blockchain in C++ version.

Make blockchain as configurable software.

## Rationale

```
+--------------------------+
|                          |                    +--------------------+
|        Blockchain        | <----------------> |                    |
|                          |                    |                    |
|  +--------------------+  |                    |   Smart Contract   |
|  |                    |  |                    |                    |
|  | Contract Libraries | <-------------------> |                    |
|  |                    |  |                    +--------------------+
|  +--------------------+  |
+--------------------------+
```

 - Blockchain invoke Lua smart contact, push necessary parameters, include fee limit, global variable `msg` table {sender, address, value, height} and smart contract saved variable `contract` table if exists.
 - Smart contract executes Lua bytecode and stops if bytecode execute finished or fee limit run out.
 - When needed, smart contract call blockchain provided smart contract libraries.
 - When smart contract stops, blockchain saved smart contract global variable `contract` if exists. 

## Instruction CALL Drops Cost

| OP_CODE  | Cost(Drops)  |  Memo |
| ------------ | ------------ | ------------ |
| OP_MOVE  |  1 |   |
| OP_LOADK | 1  |   |
| OP_LOADKX  | 1  |   |
| OP_LOADBOOL | 1  |   |
| OP_LOADNIL | 1  |   |
| OP_GETUPVAL  | 1  |   |
| OP_GETTABUP  | 2  |   |
| OP_GETTABLE | 2  |   |
| OP_SETTABUP |  2 |   |
| OP_SETUPVAL | 2  |   |
| OP_SETTABLE | 2  |   |
| OP_NEWTABLE | 1  |   |
| OP_SELF |  1 |   |
| OP_ADD |  1 |   |
| OP_SUB | 1  |   |
| OP_MUL | 4  |   |
| OP_MOD | 2  |   |
| OP_POW  | 4  |   |
| OP_DIV | 4  |   |
| OP_IDIV  |  4 |   |
| OP_BAND  | 1  |   |
| OP_BOR  | 1  |   |
| OP_BXOR | 1  |   |
| OP_SHL  |  1 |   |
| OP_UNM | 1  |   |
| OP_BNOT | 1  |   |
| OP_NOT  | 1  |   |
| OP_LEN |  2 |   |
| OP_CANCAT | 3  |   |
| OP_JMP | 1  |   |
| OP_EQ  | 1  |   |
| OP_LT  | 1  |   |
| OP_LE  | 1  |   |
| OP_TEST |  1 |   |
| OP_TESTSET | 1  |   |
| OP_CALL  | 6  |   |
| OP_TAILCALL | 8  |   |
| OP_RETURN  | 2  |   |
| OP_FORLOOP | 4  |   |
| FORPREP |  4 |   |
| OP_TFORCALL  |  6 |   |
| OP_TFORLOOP  | 6  |   |
| OP_SETLIST  | 1  |   |
| OP_CLOSURE  | 2  |   |
| OP_VARARG  | 2 |   |
| OP_EXTRAARG |  2 |   |


## Tailored Lib

We tailored io, fs and coroutine in Lua.

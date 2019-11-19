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
 - When smart contract stops, blockchain save smart contract global variable `contract` if exists. 

## Instruction CALL Drops Cost

| OP_CODE  | Cost(Drops)  |  Memo |
| ------------ | ------------ | ------------ |
| OP_MOVE  |  1 |   |
| OP_LOADK | 1  |   |
| OP_LOADKX  | 1  |   |
| OP_LOADBOOL | 1  |   |
| OP_LOADNIL | 1  |   |
| OP_GETUPVAL  | 1  |   |
| OP_GETTABUP  | 1  |   |
| OP_GETTABLE | 1  |   |
| OP_SETTABUP |  2 |   |
| OP_SETUPVAL | 2  |   |
| OP_SETTABLE | 2  |   |
| OP_NEWTABLE | 2  |   |
| OP_SELF |  3 |   |
| OP_ADD |  2 |   |
| OP_SUB | 2  |   |
| OP_MUL | 2  |   |
| OP_MOD | 2  |   |
| OP_POW  | 4  |   |
| OP_DIV | 4  |   |
| OP_IDIV  |  4 |   |
| OP_BAND  | 2  |   |
| OP_BOR  | 2  |   |
| OP_BXOR | 2  |   |
| OP_SHL  |  2 |   |
| OP_SHR  | 2  |   |
| OP_UNM | 2  |   |
| OP_BNOT | 2  |   |
| OP_NOT  | 2  |   |
| OP_LEN |  16 |   |
| OP_CANCAT | 3  |   |
| OP_JMP | 3  |   |
| OP_EQ  | 2  |   |
| OP_LT  | 2  |   |
| OP_LE  | 2  |   |
| OP_TEST |  1 |   |
| OP_TESTSET | 2  |   |
| OP_CALL  | 5  |   |
| OP_TAILCALL | 20  |   |
| OP_RETURN  | 3  |   |
| OP_FORLOOP | 4  |   |
| FORPREP |  6 |   |
| OP_TFORCALL  |  6 |   |
| OP_TFORLOOP  | 4  |   |
| OP_SETLIST  | 12  |   |
| OP_CLOSURE  | 8  |   |
| OP_VARARG  | 8 |   |
| OP_EXTRAARG |  1 |   |


## Tailored Lib

We tailored io, fs and coroutine in Lua.

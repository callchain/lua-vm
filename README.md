# Lua Virtual Machine

A Lua virtual machine for blockchain in C/C++ version. Blockchain embed this Lua virtual machine to execute Lua smart contract,  make blockchain as configurable system. Also blockchain can provide syscall for Lua smart contract to interact with each others.

In this Lua virtual machine, io, thread and coroutine libraries are removed, only support basic lua functions.


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

- Blockchain call Lua smart contract, push global parameters including `fee limit`, global variable `msg` table as  {`sender`, `address`, `value`, `height`}  and smart contract saved variable `contract`.
- Lua smart contract execute Lua bytecode and stops when bytecode execute finished or contract fee limit run out.
- Smart contract may call blockchain system call libraries when it is necessary.
- Smart contract return one integer to indicate success or failure, when return 0, it is success, otherwise it is failure.
- After smart contract stop, blockchain automatic save smart contract's global variable `contract` if it is modified.

## Fee Cost

For Turing system, it should be stoppable. In blockchain it also should use fee limit to stop smart contract otherwise smart contract will hack down blockchain. In Lua virtual machine, the fee cost include memory cost and instruction cost. Memory cost is used to limit memroy usage, instruction cost is used to limit instruction steps.

### Memory Fee Cost

In Lua virtal machine, smart contract memory used is divided as memory unit. In this virtual machine, the memroy unit is 128 bytes as `CONTRACT_MEM_UNIT_SIZE`. Each memory unit has memory price. The memroy price is 2 CALL drops as `CONTRACT_MEN_UNIT_DROP_COST`.

###  Instruction Fee Cost

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



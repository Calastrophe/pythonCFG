# What is pythonCFG?
It is an easy-to-use library to implement control flow graph generation in your emulator.

All that is required is to wrap your instruction set around the two classes Instruction or Jump.



# Downloading the library
If you wish to download the library and use it, you must install Graphviz and add it to your PATH at install.

https://graphviz.org/download/

After installing Graphviz, you can use pip to install the library for use in your own emulator.

``pip install pythonCFG``

# Using the library
Assuming you have an emulator which contains some pseudocode such as...
``emulator.execute(instruction)``

You will take this instruction and wrap it into a Instruction or Jump class.
An instruction can optionally take an operand, but requires a name.

A jump requires a name and "success_address" (operand) for all types of JumpType. A failure address is needed in the case of a JCC.

Some psuedocode to simulate this is...

```py
match instruction:
      case INC:
            return Instruction("INC")
            
      case JUMP:
            return Jump("JUMP", 0x30, JumpType.JMP) ## NOTE: A failure address is not needed as this is an absolute jump!
            ## NOTE: You will need to dynamically determine what your success_address and failure_address.
            
      case CONDITIONAL_JUMP:
            return Jump("COND", 0x30, """ JumpType.JCC_TAKEN or JumpType.JCC_NOT_TAKEN """, 0x20) ## NOTE: A failure address is needed as this is conditional.
            ## It is burden upon you to determine if the jump is taken or not as it is not feasible for this library and its goals.
```

After matching your instruction set into an Instruction or Jump, you will need to execute this instruction in the graph.
Some pseudocode after you've matched your instruction set with its respective operands.
```py
    class Emulator:
        def __init__(self):
              ## OTHER ARGUMENTS ARE IMPLEMENTATION SPECIFIC
              self.graph = ControlFlowGraph(0) ## Import this class and set your entry point address ( in this case 0 ).
              
        def execute(self, instruction):
              self.graph.execute( matched_instruction(instruction) ) 
              ## We are assuming you have correctly determined operands in matched_instruction.
              
        def output(self):
              self.graph.png("some_output.png") # Returns an image of the control flow graph.
```

# Implementations?

There is an example implementation in the source code of pythonRSC.

[https://github.com/Calastrophe/pythonRSC-dev/blob/master/src/pythonRSCdev/emulator.py#L41](https://github.com/Calastrophe/pythonRSC/blob/5fded34c159ac6f31cf332b0fcd9f55314d3434d/src/pythonRSC/emulator.py#L175-L188)

Then the matched instruction is executed inside the cycle() function above it.


# What does it look like?

A large execution graph will typically look something like this ( instructions in the block depend on your architecture ).

There a few things subject to change on how the control flow graph will look in coming versions in order to better analyze the graph.

![image](https://user-images.githubusercontent.com/74928681/215513107-28c75e94-7209-4958-a671-c1cc742acc5c.png)

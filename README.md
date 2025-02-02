# AXI-FIFO-in-VHDL

<body>
    <h2>AXI FIFO State Diagram</h2>
    
    <h3>States:</h3>
    
    <h4>IDLE</h4>
    <pre>
    Condition: rst = '1' or count = 0
    Transitions:
    - If rst = '0' and in_valid = '1' → Move to WRITE
    - If rst = '0' and out_ready = '1' → Move to READ
    </pre>

    <h4>WRITE</h4>
    <pre>
    Condition: in_valid = '1' and in_ready = '1'
    Actions:
    - Store in_data into ram(head), increment head
    - If count = ram_depth - 1, move to FULL
    - If out_ready = '1' at the same time, move to SIMULTANEOUS_READ_WRITE
    - Otherwise, stay in WRITE or move to IDLE if in_valid = '0'
    </pre>

    <h4>READ</h4>
    <pre>
    Condition: out_ready = '1' and out_valid = '1'
    Actions:
    - Output ram(tail) to out_data, increment tail
    - If count = 0, move to EMPTY
    - If in_valid = '1' at the same time, move to SIMULTANEOUS_READ_WRITE
    - Otherwise, stay in READ or move to IDLE if out_ready = '0'
    </pre>

    <h4>FULL</h4>
    <pre>
    Condition: count = ram_depth - 1
    Actions:
    - in_ready = '0', prevents new writes
    - If out_ready = '1', move to READ
    - If rst = '1', move to IDLE
    </pre>

    <h4>EMPTY</h4>
    <pre>
    Condition: count = 0
    Actions:
    - out_valid = '0', prevents reads
    - If in_valid = '1', move to WRITE
    - If rst = '1', stay in IDLE
    </pre>

    <h4>SIMULTANEOUS_READ_WRITE</h4>
    <pre>
    Condition: in_valid = '1' and out_ready = '1'
    Actions:
    - Increment both head and tail
    - If count = 1, move to EMPTY
    - If count = ram_depth - 1, move to FULL
    - Otherwise, move to WRITE or READ depending on input conditions
    </pre>
</body>
</html>

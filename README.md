# AXI-FIFO-in-VHDL
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AXI FIFO State Diagram</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            padding: 20px;
            background-color: #f9f9f9;
        }
        h2 {
            color: #2E86C1;
        }
        .state {
            background: #f4f4f4;
            padding: 10px;
            border-radius: 5px;
            margin-bottom: 10px;
            white-space: pre-wrap;
            word-wrap: break-word;
        }
    </style>
</head>
<body>
    <h2>AXI FIFO State Diagram</h2>
    
    <div class="state">
        <h3>IDLE</h3>
        <p><strong>Condition:</strong> rst = '1' or count = 0</p>
        <p><strong>Transitions:</strong></p>
        <ul>
            <li>If rst = '0' and in_valid = '1' → Move to WRITE</li>
            <li>If rst = '0' and out_ready = '1' → Move to READ</li>
        </ul>
    </div>

    <div class="state">
        <h3>WRITE</h3>
        <p><strong>Condition:</strong> in_valid = '1' and in_ready = '1'</p>
        <p><strong>Actions:</strong></p>
        <ul>
            <li>Store in_data into ram(head), increment head</li>
            <li>If count = ram_depth - 1, move to FULL</li>
            <li>If out_ready = '1' at the same time, move to SIMULTANEOUS_READ_WRITE</li>
            <li>Otherwise, stay in WRITE or move to IDLE if in_valid = '0'</li>
        </ul>
    </div>

    <div class="state">
        <h3>READ</h3>
        <p><strong>Condition:</strong> out_ready = '1' and out_valid = '1'</p>
        <p><strong>Actions:</strong></p>
        <ul>
            <li>Output ram(tail) to out_data, increment tail</li>
            <li>If count = 0, move to EMPTY</li>
            <li>If in_valid = '1' at the same time, move to SIMULTANEOUS_READ_WRITE</li>
            <li>Otherwise, stay in READ or move to IDLE if out_ready = '0'</li>
        </ul>
    </div>

    <div class="state">
        <h3>FULL</h3>
        <p><strong>Condition:</strong> count = ram_depth - 1</p>
        <p><strong>Actions:</strong></p>
        <ul>
            <li>in_ready = '0', prevents new writes</li>
            <li>If out_ready = '1', move to READ</li>
            <li>If rst = '1', move to IDLE</li>
        </ul>
    </div>

    <div class="state">
        <h3>EMPTY</h3>
        <p><strong>Condition:</strong> count = 0</p>
        <p><strong>Actions:</strong></p>
        <ul>
            <li>out_valid = '0', prevents reads</li>
            <li>If in_valid = '1', move to WRITE</li>
            <li>If rst = '1', stay in IDLE</li>
        </ul>
    </div>

    <div class="state">
        <h3>SIMULTANEOUS_READ_WRITE</h3>
        <p><strong>Condition:</strong> in_valid = '1' and out_ready = '1'</p>
        <p><strong>Actions:</strong></p>
        <ul>
            <li>Increment both head and tail</li>
            <li>If count = 1, move to EMPTY</li>
            <li>If count = ram_depth - 1, move to FULL</li>
            <li>Otherwise, move to WRITE or READ depending on input conditions</li>
        </ul>
    </div>
</body>
</html>

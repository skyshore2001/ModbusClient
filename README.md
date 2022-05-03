# ModbusClient

A php lib to read/write PLC via modbus-tcp protocol.

Usage (level 1): read/write once (short connection)

```php
require("common.php");
require("ModbusClient.php");
try {
	// S1.0:dword - slave1, addr 0 (NOTE: word addr from 0)
	ModbusClient::writePlc("192.168.1.101", [["S1.0:dword", 70000], ["S2.0:word[2]", [30000,50000]], ["S3.0:float", 3.14]]);

	$res = ModbusClient::readPlc("192.168.1.101", ["S1.0", "S2.0:word[2]", "S3.0:float"]);
	var_dump($res);
	// on success $res=[ 70000, [30000,50000], 3.14 ]
}
catch (ModbusException $ex) {
	echo($ex->getMessage());
}
```

Usage (level 2): read and write in one connection (long connection)

```php
try {
	$plc = new ModbusClient("192.168.1.101"); // default tcp port 502: "192.168.1.101:502"
	$plc->write([["S1.0:dword", 70000], ["S2.0:word[2]", [30000,50000]], ["S3.0:float", 3.14]]);
	$res = $plc->read(["S1.0:dword", "S2.0:word[2]", "S3.0:float"]);
	var_dump($res);
	// on success $res=[ 70000, [30000,50000], 3.14 ]
}
catch (ModbusException $ex) {
	echo($ex->getMessage());
}
```

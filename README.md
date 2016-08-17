go-kcps2
=============
*CAUTION: UNOFFICIAL*  

A [KCPS2](http://www.kddi.com/business/cloud/iaas-paas-daas/cloud-platform/) API client enabling Go programs to interact with KCPS2.
This package is based on [xanzy/go-cloudstack](https://github.com/xanzy/go-cloudstack).

## Description
This package covers the complete [KCPS2 API](https://manual.cloud-platform.kddi.ne.jp/developer/api/cloud-stack-api/list/).

## Features
* two clients (created with NewClient(...) and  NewAsyncClient(...)) function and create the needed API command parameter struct a New...Params function reference [here](https://github.com/xanzy/go-cloudstack#features)
* This package because it is based on [xanzy/go-cloudstack](https://github.com/xanzy/go-cloudstack) , other features is also based . See [here](https://github.com/xanzy/go-cloudstack#features)

## Example
```go
package main

import (
	"encoding/json"
	"io/ioutil"
	"log"

	"github.com/tsubauaaa/go-kcps2/kcps2"
)

// Config is a structure of authentication information of cloudstack
type Config struct {
	Endpoint  string `json:"endpoint"`
	Apikey    string `json:"apikey"`
	Secretkey string `json:"secretkey"`
}

func main() {
	serviceofferingid := "e3060950-1b4f-4adb-b050-bbe99694da19" // Small1(1vCPU,Mem2GB) EAST-Site Value
	templateid := "7dd6b0b4-ef01-4580-aa69-7276361c892d"        // CentOS6.5(64bit)100GB
	zoneid := "48889c46-dd04-4a50-af06-8aad7ea46f61"            // jp2-east01
	hypervisor := "VMware"
	networkids := []string{"e3337415-e7fe-43ab-8779-d84ac2d78c1b"} // PublicFrontSegment
	name := "web-server1"                                          // VirtualMachine's Name

	// Load configuration file that the end point and the api key and the secret key was written
	file, _ := ioutil.ReadFile("./conf/config.json")

	var conf Config
	json.Unmarshal(file, &conf)

	client := kcps2.NewAsyncClient(conf.Endpoint, conf.Apikey, conf.Secretkey, true)
	client.HTTPGETOnly = true

	vm := kcps2.NewVirtualMachineService(client)
	params := vm.NewDeployValueVirtualMachineParams(serviceofferingid, templateid, zoneid, hypervisor, networkids, name)

	// Response that has been structured
	res, _ := vm.DeployValueVirtualMachine(params)
	log.Printf("Deploy completion: Name=%s IPAddr=%s Password=%s VMID=%s", res.Name, res.Nic[0].Ipaddress, res.Password, res.ID)
}
```

## Installation
```bash
$ go get github.com/tsubauaaa/go-kcps2/kcps2
```

## Contribution
* fork it
* develop you want
* create a pull-request !

## License
[Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0)

## Author
[tsubauaaa](https://github.com/tsubauaaa)

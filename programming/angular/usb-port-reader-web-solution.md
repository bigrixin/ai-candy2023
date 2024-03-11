# USB Port reader Web solution



{% code fullWidth="true" %}
````
// TS

```typescript
import { Component,  Inject, OnDestroy, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
import { Router } from '@angular/router';
import { RfidService } from 'src/app/services/rfid.service';
import { MatDialog, MAT_DIALOG_DATA } from '@angular/material/dialog';
import { VehicleService } from 'src/app/services/vehicle.service';
import { CustomerService } from 'src/app/services/customer.service';
import { ToastrService } from 'ngx-toastr';
import { Connect, DisConnect, CheckStatus} from '../../../../assets/script/portscript.js'


@Component({
  selector: 'app-rfid-add-update',
  templateUrl: './rfid-add-update.component.html',
  styleUrls: ['./rfid-add-update.component.scss']
})
export class RfidAddUpdateComponent implements OnInit,OnDestroy  {

  frm!: FormGroup;
  submitClicked = false;
  is_edit: boolean = true;
  VehicleList: any;
  DriverList:any;
  CompanyList: any;

  constructor(
    @Inject(MAT_DIALOG_DATA) public data:any,
    public router: Router,
    public fb: FormBuilder,
    private rfidService: RfidService,
    private dialog: MatDialog,
    private vehicleService: VehicleService,
    private customerService: CustomerService,
    private toastr: ToastrService,
  ) { }



  ngOnInit(): void {
    this.initialForm(this.data);
 //   this.getCompanyList();
    this.getVehicleList();
   CheckStatus();

  }



  initialForm(data: any){
    this.frm = this.fb.group({
      rfidid: [{value:'', disabled: !this.is_edit}],
      rfidNo: ['', Validators.required],
      serialNo: ['', Validators.required],
      vehicleid: ['', Validators.required],
    });

   if (data.action == 'Edit' )
    {
      this.frm = this.fb.group({
        rfidid: data.rfid.rfidid,
        rfidNo: data.rfid.rfidNo,
        serialNo: data.rfid.serialNo,
        vehicleid: data.rfid.vehicleid
      });
    }
  }

  onConnect() {
    Connect();
  }

  onDisConnect() {
    DisConnect();
  //  window.location.reload();
  }

  ngOnDestroy(): void {
    window.location.reload();
  }

  async onSubmit(frm:FormGroup) {
    this.submitClicked = true;
    if (this.data.action == 'New')
    {
       this.rfidService.addNewRfid(frm.value)
          .subscribe({
            next: data => {
              if (data.status == 200)
              {
                  this.toastr.success('A new record has added successfully !', 'Success',{timeOut: 5000});
                  this.dialog.closeAll();    ///close DialogComponent
                  this.reload(this.router.url);
              }
            },
            error: error => {
                console.error('There was an error in update!', error);
                this.toastr.error('Add Fail', 'Error')
          }})
      // console.log(result);
    }
    else if (this.data.action == 'Edit') {
        this.rfidService.editRfid(frm.value)
         .subscribe({
            next: data => {
             if (data.status == 200)
             {
                 this.toastr.success('Update Success', 'Success');
                 this.dialog.closeAll();    ///close DialogComponent
                // this.reload(this.router.url);
             }
            },
            error: error => {
                console.error('There was an error in update!', error);
                this.toastr.error('Update Fail', 'Error')
        }})
    }
  }

	async reload(url: string): Promise<boolean> {
		await this.router.navigateByUrl('weighing', { skipLocationChange: true });
		return this.router.navigateByUrl(url);
	}


  //#region dropdown list

  getCompanyList(){
    return this.customerService.getCompanyWithCustomerList()
      .subscribe({
        next: res=>{
        this.CompanyList = res;
      },
      error: error => {
        console.log("retrive Customer list error"+error);
    }});

  }

  getVehicleList(){
    return this.vehicleService.getAllVehicles()
      .subscribe({
        next: res=>{
          this.VehicleList = res;
        },
        error: error => {
          console.log("Error: retrive Vehicle list error"+error);
    }});
  }

  //#endregion

}

```
````
{% endcode %}

{% code fullWidth="true" %}
````
// UI

```html
<h2 mat-dialog-title>{{data.action}} Rfid </h2>
<button (click)="onConnect()" class="btn btn-outline-success btn-sm" id="connectButton">Connect RFID reader</button> &nbsp;
<button (click)="onDisConnect()" class="btn btn-outline-warning btn-sm" id="disconnectButton"  >Disconnect</button>

<label class="text-info"  style="padding-left: 100px;" id="PortStatus" >Not connected</label>
<br>
<!-- <div class="form-group mb-2">
  <textarea id="monitor" readonly rows="2" cols="61"></textarea>
</div> -->


<hr><br>
<form [formGroup] = "frm" (ngSubmit)="onSubmit(frm)">
    <mat-dialog-content >
      <div class="form-group" hidden>
        <label>Rfid Id</label>
        <input class="form-control" [attr.disabled]="true"  formControlName="rfidid" >

      </div>
      <div class="form-group mb-2">
        <label>Rfid No</label>  <span style="color: red;"> *</span>
        <input class="form-control" formControlName="rfidNo" id="rfidNo" >
      </div>

      <div class="form-group mb-2">
        <label>Serial No</label> <span style="color: red;"> *</span>
        <input class="form-control" formControlName="serialNo" >
      </div>


      <div class="form-group mb-2">
        <label>Vehicle</label> <span style="color: red;"> *</span>
        <select class="form-select form-control" formControlName="vehicleid" id="inputGroupSelect03">
          <option *ngFor="let vehicle of VehicleList" [ngValue]="vehicle.vehicleid" >{{vehicle.rego}}</option>
        </select>
      </div>
      <div><span style="color: red;">*</span> is required</div>

    </mat-dialog-content>
    <div mat-dialog-actions align="center">
      <button class="btn btn-block btn-outline-secondary btn-sm" mat-button mat-dialog-close>Cancel</button>
      <span>&nbsp; &nbsp; </span> <span>&nbsp; &nbsp; </span>
      <button type="submit" [disabled] = "!frm.valid || submitClicked"  class="btn btn-outline-success btn-sm" mat-button>Save</button>
    </div>

</form>


```

````
{% endcode %}

{% code fullWidth="true" %}
````
// 
```typescript
 
  rootURL = environment.backEndAPIEndpoint;
 

  addNewRfid(formGroupData: any): Observable<any> {
    var formData = this.buildFormData(formGroupData);
    let endPoint = this.rootURL + 'api/rfid/create';
    return this.httpClient
      .post<HttpResponse<Object>>(endPoint, formData, { observe: 'response' })
      .pipe((resp) => resp);
  }

  editRfid(formGroupData: any): Observable<any> {
    var id = formGroupData['rfidid'];
    var formData = this.buildFormData(formGroupData);
    formData.append('Rfidid', id); //edit record need it,

    // for(var pair of formData.entries()) { console.log(pair[0]+ ', '+ pair[1]); }

    let endPoint = this.rootURL + 'api/rfid/edit/' + String(id);
    return this.httpClient
      .put<HttpResponse<Object>>(endPoint, formData, { observe: 'response' })
      .pipe((resp) => resp);
  }

  //#region functions

  buildFormData(formGroupData: any) {
    var formData: any = new FormData();
    formData.append(
      'RfidNo', formGroupData['rfidNo'] != null ? formGroupData['rfidNo'] : "''"
    );
    formData.append('SerialNo', formGroupData['serialNo']);
    formData.append('Customerid', formGroupData['customerid']);
    formData.append('Vehicleid', formGroupData['vehicleid']);
    return formData;
  }
```
````
{% endcode %}

```
// Some code
```

{% code fullWidth="true" %}
````
// Angular, assets/script

```javascript
let port;
let reader;
let cancel = false;
let isConnect;
let endStr = 13;
let rfidCode = "";

export async function Connect() {
  if ("serial" in navigator) {
    // alert("Your browser supports Web Serial API!");
    try {
      port = await navigator.serial.requestPort();
      await port.open({
        baudRate: 57600,
      });
      CheckStatus();
      while (port.readable) {
        reader = port.readable.getReader();
        try {
          while (true) {
            const { value, done } = await reader.read();
            if (done) {
              cancel = true;
              console.log("Canceled\n");
              break;
            }
            const foundEnd = value[value.length - 1];
            const inputValue = new TextDecoder().decode(value);
            rfidCode = rfidCode + inputValue;

            if (foundEnd === endStr) {
              add_rfid(rfidCode);
              rfidCode = "";
            }
            else {
              rfidCode = inputValue;
            }
          }
          if (cancel) break;
          reader.releaseLock();
        } catch (error) {
          console.log("[Error] Read" + error + "\n");
        } finally {
          CheckStatus();
          break;
        }
      }
    } catch (error) {
      console.log("[Error]Open" + error + "\n");
    }
  } else {
    alert(
      "Your browser does not support Web Serial API, the latest version of Google Chrome is recommended!"
    );
  }
}

//-----------------------------------------------------------------

export async function DisConnect() {
  try {
    reader.releaseLock();
    await port.close();
    console.log("Serial port closed.");
  } catch (error) {
    console.error(error);
  }
}

export function CheckStatus() {
  isConnect = false;
  var state = document.getElementById("PortStatus");

  try {
    if (port.readable) {
      state.innerHTML = "Connected.";
      document.getElementById("connectButton").disabled = true;
      document.getElementById("disconnectButton").disabled = false;
      isConnect = true;
    } else {
      state.innerHTML = "Not connected.";
      document.getElementById("connectButton").disabled = false;
      document.getElementById("disconnectButton").disabled = true;
    }
  } catch (error) {
    state.innerHTML = "Not connected.";
    document.getElementById("connectButton").disabled = false;
    document.getElementById("disconnectButton").disabled = true;
    console.log("[Error] Close" + error + "\n");
  }
}

function add_rfid(rfidCode) {
  let input = document.getElementById("rfidNo");
  input.value = rfidCode;
  input.dispatchEvent(new Event("input", { bubbles: true }));
   //beep();
   var audio = new Audio('../../../assets/sound/beep.mp3');
   audio.play();
}


//-----------------------------------------------------------------

function beep() {
  var snd = new  Audio("data:audio/wav;base64,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qJrx4KOQjahZxWbcZgztj2c49nKmkId44S71j0c8eV9yDK6uPRzx5X18eDvjvQ6yKo9ZSS6l//8elePK/Lf//IInrOF/FvDoADYAGBMGb7FtErm5MXMlmPAJQVgWta7Zx2go+8xJ0UiCb8LHHdftWyLJE0QIAIsI+UbXu67dZMjmgDGCGl1H+vpF4NSDckSIkk7Vd+sxEhBQMRU8j/12UIRhzSaUdQ+rQU5kGeFxm+hb1oh6pWWmv3uvmReDl0UnvtapVaIzo1jZbf/pD6ElLqSX+rUmOQNpJFa/r+sa4e/pBlAABoAAAAA3CUgShLdGIxsY7AUABPRrgCABdDuQ5GC7DqPQCgbbJUAoRSUj+NIEig0YfyWUho1VBBBA//uQZB4ABZx5zfMakeAAAAmwAAAAF5F3P0w9GtAAACfAAAAAwLhMDmAYWMgVEG1U0FIGCBgXBXAtfMH10000EEEEEECUBYln03TTTdNBDZopopYvrTTdNa325mImNg3TTPV9q3pmY0xoO6bv3r00y+IDGid/9aaaZTGMuj9mpu9Mpio1dXrr5HERTZSmqU36A3CumzN/9Robv/Xx4v9ijkSRSNLQhAWumap82WRSBUqXStV/YcS+XVLnSS+WLDroqArFkMEsAS+eWmrUzrO0oEmE40RlMZ5+ODIkAyKAGUwZ3mVKmcamcJnMW26MRPgUw6j+LkhyHGVGYjSUUKNpuJUQoOIAyDvEyG8S5yfK6dhZc0Tx1KI/gviKL6qvvFs1+bWtaz58uUNnryq6kt5RzOCkPWlVqVX2a/EEBUdU1KrXLf40GoiiFXK///qpoiDXrOgqDR38JB0bw7SoL+ZB9o1RCkQjQ2CBYZKd/+VJxZRRZlqSkKiws0WFxUyCwsKiMy7hUVFhIaCrNQsKkTIsLivwKKigsj8XYlwt/WKi2N4d//uQRCSAAjURNIHpMZBGYiaQPSYyAAABLAAAAAAAACWAAAAApUF/Mg+0aohSIRobBAsMlO//Kk4soosy1JSFRYWaLC4qZBYWFRGZdwqKiwkNBVmoWFSJkWFxX4FFRQWR+LsS4W/rFRb/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////VEFHAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAU291bmRib3kuZGUAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAMjAwNGh0dHA6Ly93d3cuc291bmRib3kuZGUAAAAAAAAAACU=");
  snd.play();
}


export function add_message(message) {
  let monitor = document.getElementById("monitor");
  monitor.value += message;
  monitor.schrollTop = monitor.scrollHeight;
}

async function send() {
  let text = document.getElementById("sendBox").value;
  document.getElementById("sendBox").value = "";
  const encoder = new TextEncoder();
  const writer = port.writable.getWriter();
  await writer.write(encoder.encode(text + "\n"));
  writer.releaseLock();
}

```
````
{% endcode %}

error TS7016: Could not find a declaration file for module

tsconfig.json

//

```typescript
 
  rootURL = environment.backEndAPIEndpoint;
 
​
  addNewRfid(formGroupData: any): Observable<any> {
    var formData = this.buildFormData(formGroupData);
    let endPoint = this.rootURL + 'api/rfid/create';
    return this.httpClient
      .post<HttpResponse<Object>>(endPoint, formData, { observe: 'response' })
      .pipe((resp) => resp);
  }
​
  editRfid(formGroupData: any): Observable<any> {
    var id = formGroupData['rfidid'];
    var formData = this.buildFormData(formGroupData);
    formData.append('Rfidid', id); //edit record need it,
​
    // for(var pair of formData.entries()) { console.log(pair[0]+ ', '+ pair[1]); }
​
    let endPoint = this.rootURL + 'api/rfid/edit/' + String(id);
    return this.httpClient
      .put<HttpResponse<Object>>(endPoint, formData, { observe: 'response' })
      .pipe((resp) => resp);
  }
​
  //#region functions
​
  buildFormData(formGroupData: any) {
    var formData: any = new FormData();
    formData.append(
      'RfidNo', formGroupData['rfidNo'] != null ? formGroupData['rfidNo'] : "''"
    );
    formData.append('SerialNo', formGroupData['serialNo']);
    formData.append('Customerid', formGroupData['customerid']);
    formData.append('Vehicleid', formGroupData['vehicleid']);
    return formData;
  }
```

// Some code // Angular, assets/script ​

````javascript
let port;
let reader;
let cancel = false;
let isConnect;
let endStr = 13;
let rfidCode = "";
​
export async function Connect() {
  if ("serial" in navigator) {
    // alert("Your browser supports Web Serial API!");
    try {
      port = await navigator.serial.requestPort();
      await port.open({
        baudRate: 57600,
      });
      CheckStatus();
      while (port.readable) {
        reader = port.readable.getReader();
        try {
          while (true) {
            const { value, done } = await reader.read();
            if (done) {
              cancel = true;
              console.log("Canceled\n");
              break;
            }
            const foundEnd = value[value.length - 1];
            const inputValue = new TextDecoder().decode(value);
            rfidCode = rfidCode + inputValue;
​
            if (foundEnd === endStr) {
              add_rfid(rfidCode);
              rfidCode = "";
            }
            else {
              rfidCode = inputValue;
            }
          }
          if (cancel) break;
          reader.releaseLock();
        } catch (error) {
          console.log("[Error] Read" + error + "\n");
        } finally {
          CheckStatus();
          break;
        }
      }
    } catch (error) {
      console.log("[Error]Open" + error + "\n");
    }
  } else {
    alert(
      "Your browser does not support Web Serial API, the latest version of Google Chrome is recommended!"
    );
  }
}
​
//-----------------------------------------------------------------
​
export async function DisConnect() {
  try {
    reader.releaseLock();
    await port.close();
    console.log("Serial port closed.");
  } catch (error) {
    console.error(error);
  }
}
​
export function CheckStatus() {
  isConnect = false;
  var state = document.getElementById("PortStatus");
​
  try {
    if (port.readable) {
      state.innerHTML = "Connected.";
      document.getElementById("connectButton").disabled = true;
      document.getElementById("disconnectButton").disabled = false;
      isConnect = true;
    } else {
      state.innerHTML = "Not connected.";
      document.getElementById("connectButton").disabled = false;
      document.getElementById("disconnectButton").disabled = true;
    }
  } catch (error) {
    state.innerHTML = "Not connected.";
    document.getElementById("connectButton").disabled = false;
    document.getElementById("disconnectButton").disabled = true;
    console.log("[Error] Close" + error + "\n");
  }
}
​
function add_rfid(rfidCode) {
  let input = document.getElementById("rfidNo");
  input.value = rfidCode;
  input.dispatchEvent(new Event("input", { bubbles: true }));
   //beep();
   var audio = new Audio('../../../assets/sound/beep.mp3');
   audio.play();
}
​
​
//-----------------------------------------------------------------
​
function beep() {
  var snd = new  Audio("data:audio/wav;base64,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qJrx4KOQjahZxWbcZgztj2c49nKmkId44S71j0c8eV9yDK6uPRzx5X18eDvjvQ6yKo9ZSS6l//8elePK/Lf//IInrOF/FvDoADYAGBMGb7FtErm5MXMlmPAJQVgWta7Zx2go+8xJ0UiCb8LHHdftWyLJE0QIAIsI+UbXu67dZMjmgDGCGl1H+vpF4NSDckSIkk7Vd+sxEhBQMRU8j/12UIRhzSaUdQ+rQU5kGeFxm+hb1oh6pWWmv3uvmReDl0UnvtapVaIzo1jZbf/pD6ElLqSX+rUmOQNpJFa/r+sa4e/pBlAABoAAAAA3CUgShLdGIxsY7AUABPRrgCABdDuQ5GC7DqPQCgbbJUAoRSUj+NIEig0YfyWUho1VBBBA//uQZB4ABZx5zfMakeAAAAmwAAAAF5F3P0w9GtAAACfAAAAAwLhMDmAYWMgVEG1U0FIGCBgXBXAtfMH10000EEEEEECUBYln03TTTdNBDZopopYvrTTdNa325mImNg3TTPV9q3pmY0xoO6bv3r00y+IDGid/9aaaZTGMuj9mpu9Mpio1dXrr5HERTZSmqU36A3CumzN/9Robv/Xx4v9ijkSRSNLQhAWumap82WRSBUqXStV/YcS+XVLnSS+WLDroqArFkMEsAS+eWmrUzrO0oEmE40RlMZ5+ODIkAyKAGUwZ3mVKmcamcJnMW26MRPgUw6j+LkhyHGVGYjSUUKNpuJUQoOIAyDvEyG8S5yfK6dhZc0Tx1KI/gviKL6qvvFs1+bWtaz58uUNnryq6kt5RzOCkPWlVqVX2a/EEBUdU1KrXLf40GoiiFXK///qpoiDXrOgqDR38JB0bw7SoL+ZB9o1RCkQjQ2CBYZKd/+VJxZRRZlqSkKiws0WFxUyCwsKiMy7hUVFhIaCrNQsKkTIsLivwKKigsj8XYlwt/WKi2N4d//uQRCSAAjURNIHpMZBGYiaQPSYyAAABLAAAAAAAACWAAAAApUF/Mg+0aohSIRobBAsMlO//Kk4soosy1JSFRYWaLC4qZBYWFRGZdwqKiwkNBVmoWFSJkWFxX4FFRQWR+LsS4W/rFRb////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// 
```typescript

  rootURL = environment.backEndAPIEndpoint;

​
  addNewRfid(formGroupData: any): Observable<any> {
    var formData = this.buildFormData(formGroupData);
    let endPoint = this.rootURL + 'api/rfid/create';
    return this.httpClient
      .post<HttpResponse<Object>>(endPoint, formData, { observe: 'response' })
      .pipe((resp) => resp);
  }
​
  editRfid(formGroupData: any): Observable<any> {
    var id = formGroupData['rfidid'];
    var formData = this.buildFormData(formGroupData);
    formData.append('Rfidid', id); //edit record need it,
​
    // for(var pair of formData.entries()) { console.log(pair[0]+ ', '+ pair[1]); }
​
    let endPoint = this.rootURL + 'api/rfid/edit/' + String(id);
    return this.httpClient
      .put<HttpResponse<Object>>(endPoint, formData, { observe: 'response' })
      .pipe((resp) => resp);
  }
​
  //#region functions
​
  buildFormData(formGroupData: any) {
    var formData: any = new FormData();
    formData.append(
      'RfidNo', formGroupData['rfidNo'] != null ? formGroupData['rfidNo'] : "''"
    );
    formData.append('SerialNo', formGroupData['serialNo']);
    formData.append('Customerid', formGroupData['customerid']);
    formData.append('Vehicleid', formGroupData['vehicleid']);
    return formData;
  }
````

// Some code // Angular, assets/script ​

```javascript
let port;
let reader;
let cancel = false;
let isConnect;
let endStr = 13;
let rfidCode = "";
​
export async function Connect() {
  if ("serial" in navigator) {
    // alert("Your browser supports Web Serial API!");
    try {
      port = await navigator.serial.requestPort();
      await port.open({
        baudRate: 57600,
      });
      CheckStatus();
      while (port.readable) {
        reader = port.readable.getReader();
        try {
          while (true) {
const { value, done } = await reader.read();
            if (done) {
              cancel = true;
              console.log("Canceled\n");
              break;
            }
            const foundEnd = value[value.length - 1];
            const inputValue = new TextDecoder().decode(value);
            rfidCode = rfidCode + inputValue;
​
            if (foundEnd === endStr) {
              add_rfid(rfidCode);
              rfidCode = "";
            }
            else {
              rfidCode = inputValue;
            }
          }
          if (cancel) break;
          reader.releaseLock();
        } catch (error) {
          console.log("[Error] Read" + error + "\n");
        } finally {
          CheckStatus();
          break;
        }
      }
    } catch (error) {
      console.log("[Error]Open" + error + "\n");
    }
  } else {
    alert(
      "Your browser does not support Web Serial API, the latest version of Google Chrome is recommended!"
    );
  }
}
​
//-----------------------------------------------------------------
​
export async function DisConnect() {
  try {
    reader.releaseLock();
    await port.close();
    console.log("Serial port closed.");
  } catch (error) {
    console.error(error);
  }
}
​
export function CheckStatus() {
  isConnect = false;
  var state = document.getElementById("PortStatus");
​
  try {
    if (port.readable) {
      state.innerHTML = "Connected.";
      document.getElementById("connectButton").disabled = true;
      document.getElementById("disconnectButton").disabled = false;
      isConnect = true;
    } else {
      state.innerHTML = "Not connected.";
      document.getElementById("connectButton").disabled = false;
      document.getElementById("disconnectButton").disabled = true;
    }
  } catch (error) {
    state.innerHTML = "Not connected.";
document.getElementById("connectButton").disabled = false;
    document.getElementById("disconnectButton").disabled = true;
    console.log("[Error] Close" + error + "\n");
  }
}
​
function add_rfid(rfidCode) {
  let input = document.getElementById("rfidNo");
  input.value = rfidCode;
  input.dispatchEvent(new Event("input", { bubbles: true }));
   //beep();
   var audio = new Audio('../../../assets/sound/beep.mp3');
   audio.play();
}
​
​
//-----------------------------------------------------------------
​
function beep() {
var snd = new  Audio("data:audio/wav;base64,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qJrx4KOQjahZxWbcZgztj2c49nKmkId44S71j0c8eV9yDK6uPRzx5X18eDvjvQ6yKo9ZSS6l//8elePK/Lf//IInrOF/FvDoADYAGBMGb7FtErm5MXMlmPAJQVgWta7Zx2go+8xJ0UiCb8LHHdftWyLJE0QIAIsI+UbXu67dZMjmgDGCGl1H+vpF4NSDckSIkk7Vd+sxEhBQMRU8j/12UIRhzSaUdQ+rQU5kGeFxm+hb1oh6pWWmv3uvmReDl0UnvtapVaIzo1jZbf/pD6ElLqSX+rUmOQNpJFa/r+sa4e/pBlAABoAAAAA3CUgShLdGIxsY7AUABPRrgCABdDuQ5GC7DqPQCgbbJUAoRSUj+NIEig0YfyWUho1VBBBA//uQZB4ABZx5zfMakeAAAAmwAAAAF5F3P0w9GtAAACfAAAAAwLhMDmAYWMgVEG1U0FIGCBgXBXAtfMH10000EEEEEECUBYln03TTTdNBDZopopYvrTTdNa325mImNg3TTPV9q3pmY0xoO6bv3r00y+IDGid/9aaaZTGMuj9mpu9Mpio1dXrr5HERTZSmqU36A3CumzN/9Robv/Xx4v9ijkSRSNLQhAWumap82WRSBUqXStV/YcS+XVLnSS+WLDroqArFkMEsAS+eWmrUzrO0oEmE40RlMZ5+ODIkAyKAGUwZ3mVKmcamcJnMW26MRPgUw6j+LkhyHGVGYjSUUKNpuJUQoOIAyDvEyG8S5yfK6dhZc0Tx1KI/gviKL6qvvFs1+bWtaz58uUNnryq6kt5RzOCkPWlVqVX2a/EEBUdU1KrXLf40GoiiFXK///qpoiDXrOgqDR38JB0bw7SoL+ZB9o1RCkQjQ2CBYZKd/+VJxZRRZlqSkKiws0WFxUyCwsKiMy7hUVFhIaCrNQsKkTIsLivwKKigsj8XYlwt/WKi2N4d//uQRCSAAjURNIHpMZBGYiaQPSYyAAABLAAAAAAAACWAAAAApUF/Mg+0aohSIRobBAsMlO//Kk4soosy1JSFRYWaLC4qZBYWFRGZdwqKiwkNBVmoWFSJkWFxX4FFRQWR+LsS4W/rFRb/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////VEFHAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAU291bmRib3kuZGUAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAMjAwNGh0dHA6Ly93d3cuc291bmRib3kuZGUAAAAAAAAAACU=");
  snd.play();
}
​
​
export function add_message(message) {
  let monitor = document.getElementById("monitor");
  monitor.value += message;
  monitor.schrollTop = monitor.scrollHeight;
}
​
async function send() {
  let text = document.getElementById("sendBox").value;
  document.getElementById("sendBox").value = "";
  const encoder = new TextEncoder();
  const writer = port.writable.getWriter();
  await writer.write(encoder.encode(text + "\n"));
  writer.releaseLock();
}
​
```

error TS7016: Could not find a declaration file for module tsconfig.json "compilerOptions": { "noImplicitAny":false, ... }

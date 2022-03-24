# Powershell notes #


## general operators ##

`select -first 10, select -ft 10`  show the first 10 result of something. 

`select *, format-list -property *`  show all properties of an object.  select is preferable in most cases as it doesn't modify the object.

`$resp.Content`  properties of objects can be accessed with dot operators.  this returns the result as a string.


## listing files ##
`dir, ls, gci` are all `Get-ChildItem`


`-h -H -Hidden` shows hidden folders.  this is similar but not the same as -force


`-force` shows everything the system can show without violating security. 


`-file` shows just files


`-directory` shows just folders


`-Name` prints just the names and nothing else.  use for pipeing and stuff.


`-system` show only system files.  need to use with `-force.  ls -force -system`


`-attribues, -at, -att`  returns files with the sepecifed type.  eg: `ls -force -attribues directory`  or `ls -force -attribues archive`  or `ls -for -at dir` for directories 

    - D (Directory)
    - H (Hidden)
    - R (Read-only)
    - S (System)

    - \! (NOT)
    - \+ (AND)
    - \, (OR)


`-recurse, -re`  returns all the files and subfiles of directory.  use `ls -force -re -Name`  to return everything that is accessible without printing error messages


## reading files ##

`get-content <file>, cat <file>`  read file to stdout


`get-content <file> -Tail 10`  last 10 lines


`get-content <file> -Wait -Tail 10` last 10 lines and wait for more.


## writing files ##

`new-item -Path . -Name "testfile.txt" -Itemtype "file" -Value "contents of file"`  create a new file and put some text into it


`new-item -Path . -Name "testdir" -Itemtype "directory"`  create a new directory at path


`$link = new-item -Itemtype SymbolicLink -Path . -Target ./notes.txt
$link = | select-object LinkType, Target
`  create a new symbolic link to location



### file systems ###


`get-psdrive`  show other drives.  you can then cd into them.


`get-volume`  shows other drives and their sizes and health and stuff.




## weird ones ##


`Get-Clipboard`  just returns the clipboard contents as an array of strings.  with each new line being a new string.  `-raw` to force return the literal contents. 

`get-computerinfo`  returns a ton of stuff about the actual device.  like os versions and hardware hosts.


`.gettype()`  shows the powershell type of an object


`get-unique`  shows unique entries, but, it MUST be sorted first


`get-windowsfeature`  shows the installed services and stuff.



## users and groups ##


`get-localuser`  shows users  `get-localuser | select *` shows all available information


## services ##


`get-service`  show all installed services and their status.  `get-service | Sort-Object status -D`  shows running on top.


`start-service -Name "sshd"`  start a service


`set-service -Name sshd -StartupType Automatic`  set aspects of services.  Display name, description, status, startuptype. 


`set-service -Name sshd -Status Running -PassThru`


## networking ##


`get-nettcpconnection`  show open tcp connections.




### firewall ###


`get-netfirewallprofile -Name Public`  show profile settings like default actions.


`set-netfirewallprofile -All -Enabled True`  turn on all three firewalls.


`set-netfirewallprofile -Name Public -DefaultInboundAction Block`  set default to block



`get-netfirewallportfilter | where localport -eq 22 | get-netfirewallrule`  search the port rules for something on local port 22 and return the rule that is associated with it.




## http connections ##

`invoke-webrequest`  aliased as `curl` and `wget`


`$resp = curl -URI http://localhost:8080
$resp
`  make simple get request


`$params = {username='uname'; moredat='dat'}
curl -Uri http://localhost:8080 -Method POST -Body $params
`  make a post request 
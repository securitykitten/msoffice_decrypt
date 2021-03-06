# msoffice_decrypt
msoffice_decrypt is a Python tool and library for decrypting encrypted MS
Office files with a password.  This work is based on
<https://github.com/nolze/msoffcrypto-tool>. I created this project to solves a
specific use case for myself.

## Examples
### (command line) Decrypt a file with a password.

```
./msoffice_decrypt.py -p 7779 Scan_ciwilson.doc Scan_ciwilson_unencrypted.doc
```
```
decrypted Scan_ciwilson.doc into Scan_ciwilson_unencrypted.doc
```

### (command line) Decrypt a file trying everything that might be a password in this other file.
```
./msoffice_decrypt.py -i sample.txt Scan_ciwilson.doc Scan_ciwilson_unencrypted.doc
```

```
found password: 7779
decrypted Scan_ciwilson.doc into Scan_ciwilson_unencrypted.doc
```

### (library) 
```python
from msoffice_decrypt import MSOfficeDecryptor
decryptor = MSOfficeDecryptor(input_file_path, output_file_path)
if decryptor.is_decryptable:
    # generate a list of passwords that might be right
    # here we assume sample.txt is a text file that contains the password somewhere
    with open('sample.txt', 'r') as fp:
        word_list = decryptor.find_password(fp)

    # see if any of these passwords are correct
    password = decryptor.guess(word_list)

    if password:
        decryptor.decrypt(password)
```


the provided code of the java files is a compiled one we need to decompile them to get the necessary code 
# LEARN JAVA 
The flag is : flag{KnoW_YouR_A5C115}


byte[] var3 = new byte[]{75, 110, 111, 87, 95, 89, 111, 117, 82, 95, 65, 53, 67, 49, 49, 53, 125};


we need to convert each ASCII value into its corresponding character


this gives us KnoW_YouR_A5C115} 


String var4 = var3.substring("flag{".length(), var3.length());


we need to add the flag{ part as the rest to get the final password
 
# ENCRYPT FLAG

The flag is : flag{Simple_XOR_4qfW33}

The yes method in the provided code used a simple XOR encryption with the key "3". so i decrypted the encrypted part (G[Z@Z@G[VU_RT)of the flag by XORing each character with "3." this will give  Simple_XOR_

The no method involved concatenating two strings, calculating their hash code, and converting the hash code into a base-62 representation. reversing this entire process will give us 4qfW33

finally, construct the final flag by enclosing and concatenating these strings in flag{}


# ASSEMBLY CRASH COURSE CHALLENGES
i could only solve the first two challenges of this . 

## level 1 

we can use the move function to store the value to a register
syntax : mov $0x1337,%rdi


$ - the following value is a immediate value 


%- we are storing the value into a register

## level 2

we can use the move command here also

syntax:

mov $0x1337, %rax

mov $0xCAFED00D1337BEEF, % r12


mov $0x31337 ,%rsp






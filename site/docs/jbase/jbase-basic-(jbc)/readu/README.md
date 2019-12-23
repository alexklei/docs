# READU

**Created At:** 9/28/2017 6:47:02 AM  
**Updated At:** 10/27/2018 10:08:33 AM  
**Original Doc:** [278774-readu](https://docs.jbase.com/36868-jbase-basic/278774-readu)  


# Description

The **READU** statement allows a program to read a record from a previously opened file into a variable. It respects record locking and locks the specified record for update.

It takes the general form:

```
READU variable1 FROM {variable2,} expression {SETTING setvar} 
{ON ERROR statements} {LOCKED statements} {THEN|ELSE statements}
```

Where:

**variable1**is the identifier into which the record will be read.

**variable2**if specified, should be a jBASE BASIC variable that has previously been opened to a file using the [OPEN](277537-open) statement. If variable2 is not specified then the default file is assumed.

The **expression**should evaluate to a valid record key for the file.

If the **SETTING**clause is specified and the read fails, **setvar**will be set to one of [these values](277647-increamental-file-errors).

If **ON ERROR** is specified, the statements following the **ON ERROR** clause will be executed for any of the above Incremental File Errors except error 128.

If the **READU** is successful then the statements following **THEN**will be executed. If the **READU** is unsuccessful, i.e. the record key does not exist in the file, then the statements following **ELSE**are executed. If the **READU** is unsuccessful and there is no **ELSE**then **expression** is set to "" (null).

## Note: 


> If the record could not be read because another process already had a lock on the record then one of two actions is taken. If the LOCKED clause was specified in the statement then the statements dependent on it are executed. If no LOCKED clause was specified then the statement blocks (waits) until the other process releases the lock. The [SYSTEM(43)](282982-system-functions) function can be used to determine which port has the lock.
> 
> The lock taken by the**READU**statement will be released by any of the following events:
> 
> - The same program with [WRITE](279568-write), [WRITEV](279574-writev)or [MATWRITE](276964-matwrite) statements writes to the record.
> - The same program with the [DELETE](276025-delete) statement deletes the record.
> - The record lock is released explicitly using the [RELEASE](278784-release) statement.
> - The program stops normally or abnormally.
> 
> 
> When a file is [OPEN](277537-open)ed to a local file variable in a subroutine then the file is closed when the subroutine [RETURN](278787-return)s so all locks taken on that file are released, including locks taken in a calling program. Files that are opened to [COMMON](276024-common) variables are not closed so the locks remain intact.


An example of use is as:

```
OPEN "Customers" ELSE ABORT 201, "Customers"
OPEN "DICT Customers" TO DCusts ELSE ABORT 201, "DICT Customers"
END
LOOP
    READU Rec FROM DCusts, "Xref" LOCKED
        CRT "Xref locked by port ":SYSTEM(43):" - retrying"
        SLEEP 1; 
        CONTINUE ;* Restart LOOP
    END THEN
    READ DataRec FROM Rec ELSE 
        ABORT 202, Rec
        END
        BREAK ;* Leave the LOOP
    END ELSE
    ABORT 202, "Xref"
    END
REPEAT
```





See also: [WRITE](279568-write), [WRITEU](279573-writeu), [MATWRITE](276964-matwrite), [MATWRITEU](276970-matwriteu), [RELEASE](278784-release), [DELETE](276025-delete).

Go back to [jBASE BASIC](263498-jbase-basic).
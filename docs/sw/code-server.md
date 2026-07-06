## Run code-server interactively on JHPCE (an alternative way to run vscode/vs-code via the web portal)

1. Run the following commands after login to JHPCE
```
[test@jhpce01 ~]$ srun --pty bash
[test@compute-149 ~]$ codeserver.sh 
        ==================================================================================
        Starting code-server

        Please type the following on your local terminal
            ## Note: Windows users, please use PowerShell
            ssh -J test@jhpce01.jhsph.edu -N -L 40000:127.0.0.1:40000 test@compute-149

        Then open:
        http://127.0.0.1:40000

        ====================================================================================
[2026-05-04T19:43:21.633Z] info  code-server 4.112.0 d7599ae360900ad55b503e3c840b417a1eae4798
[2026-05-04T19:43:21.646Z] info  Using user-data-dir /users/test/.local/share/code-server
[2026-05-04T19:43:21.662Z] info  Using config file /users/test/.config/code-server/config.yaml
[2026-05-04T19:43:21.662Z] info  HTTP server listening on http://127.0.0.1:40008/
[2026-05-04T19:43:21.662Z] info    - Authentication is disabled
[2026-05-04T19:43:21.662Z] info    - Not serving HTTPS
[2026-05-04T19:43:21.662Z] info  Session server listening on /users/test/.local/share/code-server/code-server-ipc.sock
```

2. Set up port forward - open a terminal on your local machine, copy and paste line from the output of codeserver.sh
```
uname@stone ~ % ssh -J test@jhpce01.jhsph.edu -N -L 40000:127.0.0.1:40000 test@compute-149
```

3. Open a browser on your local machine, then copy and paste the url from the output of your codeserver.sh
```
http://127.0.0.1:40000
```

Note: The code-server is now available to use through your browser. Once you have done, type ctrl-c on both JHPCE terminal and your local terminal, and close the browser.


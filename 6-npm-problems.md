To solve `bower : The term 'bower' is not recognized as the name of a cmdlet, function, script file, or operable program.`

Use ```npm install -g bower```

<img width="608" alt="33342fce062d93775da7375d2c102eb" src="https://user-images.githubusercontent.com/74582280/227071717-a728c9d3-c9e3-48d8-bd99-9a33bad4ca23.png">

To solve `bower: .../.../.../ cannot be loaded.`

Use ```Get-ExecutionPolicy```

Then ```Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser```

refer to: https://techblog.dev/posts/2022/10/fix-bower-ps1-cannot-be-loaded-running-scripts-disabled-error-windows-powershell/

<img width="603" alt="df2cb241d7da7d406cc46ee44abaf04" src="https://user-images.githubusercontent.com/74582280/227071743-0485cd9b-14fd-4c4c-a892-305191af5d0b.png">

<img width="601" alt="eac93834f595176a0ac0ac821405790" src="https://user-images.githubusercontent.com/74582280/227071757-b1465ab1-535a-4e07-8d58-777384c6dc52.png">


---
tags:
  - daily
---
--------
## Task to complete

- [ ] 
- [ ]   

-----
##  Action items from daily meeting

| Task | Further Action item |
| ---- | ------------------- |
|      |                     |


----

## Notes


To clear the URL cache in the Microsoft Teams desktop app, you must delete the local cache by first fully quitting Teams, then opening the Run dialog (Windows key + R) and entering %appdata%\Microsoft\Teams or the newer Teams path %userprofile%\appdata\local\Packages\MSTeams_8wekyb3d8bbwe\LocalCache\Microsoft\MSTeams, selecting all files, and deleting them. Restarting Teams will then rebuild its cache, resolving most persistent URL issues. [1, 2, 3, 4, 5]  

For Windows Users

1. Quit Teams: Right-click the Microsoft Teams icon in your system tray and select Quit to ensure the application is fully closed. [1]  

2. Open Run Dialog: Press the Windows key + R on your keyboard simultaneously to open the Run dialog box. [2, 3]  

3. Navigate to Cache Folder:

• For the new Teams app: Enter %userprofile%\appdata\local\Packages\MSTeams_8wekyb3d8bbwe\LocalCache\Microsoft\MSTeams and click OK. [2, 3, 6]  

• For the classic Teams app: Enter %appdata%\Microsoft\Teams and click OK. [1, 7]  

4. Delete Cache Files: In the opened folder, press Ctrl + A to select all files and folders, then press the Delete key or right-click and select Delete. [2, 3]  

5. Relaunch Teams: Open Microsoft Teams again; it will take longer to start as it rebuilds the cache. [3]  

This video demonstrates how to clear the cache for Microsoft Teams on Windows: [https://www.youtube.com/watch?v=uG792FQXpRo](https://www.youtube.com/watch?v=uG792FQXpRo "https://www.youtube.com/watch?v=ug792fqxpro") ([https://www.youtube.com/watch?v=uG792FQXpRo](https://www.youtube.com/watch?v=uG792FQXpRo "https://www.youtube.com/watch?v=ug792fqxpro"))

For Mac Users [1, 6, 8, 9]  

1. Quit Teams: In Finder, select Go &gt; Applications, then right-click Microsoft Teams and choose Quit.

2. Open Terminal: Open the /Applications/Utilities folder and double-click on Terminal.

3. Execute Commands: Enter the following commands, pressing Return after each:

• rm -rf ~/Library/Group Containers/UBF8T346G9.com.microsoft.teams

• rm -rf ~/Library/Containers/com.microsoft.teams2

4. Restart Teams: Re-launch Microsoft Teams to clear the cache.

AI responses may include mistakes.






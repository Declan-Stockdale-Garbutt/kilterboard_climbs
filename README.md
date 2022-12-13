# Kilterboard climbs

Ths project is aimed at scraping climbs on the KilterBoard and ultimately using machine learning to create new high quality climbs which will benefit the climbing community

## How it works
### Bluestacks
This is an app emulator for android apps. I have logged in suing my personal credientials and downloaded the KitlerBoard app.

I was unable to load a direct programming interface into BlueStacks so I have implemetned a hacky work around using pyautogui to emulate a person swiping through the app

It works by loading the app and navigating to the searc menu. Selecting some search criteria such as nly look at boulders, no open projects and the angle whihc I chose to be 40 as it seems quite common and is similar to the MoonBoard which may offer some comparisons at a alter stage.

### Collecting the images
The climbs were loaded in and here some hacky things occured. 
Firstly A screenshot is taken of th active screen using PIL. Tesseract OCR is used to read the name of the climb which is then saved locally. Pyautogiu then emulates weiping t the right loading in a new climb. The process repeates until the app reaches some pre determined point. Initially there were over 7,000 climbs available but it appears to repeat after 2,000.

### Alternative collection method
Another option occured which involved investigating the apk of the app and finding a sqlite database. I loaded that into DBeaver and extracted all climbs at 40 degrees along with the climb name, difficulty, number of ascents and quality. The results were restricted to climbs with at least 5 ascents and had a displayed difficulty of 20 which corresponds to 6b or easy v4. The frame_count 1 restricts to boulders rather than routes which change over time

```
SELECT  climb_uuid, name,  display_difficulty, benchmark_difficulty, ascensionist_count, quality_average 
FROM climb_stats 
INNER JOIN climbs 
ON climb_stats.climb_uuid = climbs.uuid
WHERE angle = 40 and display_difficulty > 18 and ascensionist_count >=5 and frames_count =1;
```

Once I had this I filtered my saved climsb from climbs that occured in the databsae to find unsaved climbs. I then modified my existing script to loop over the name of each unsaved climb, enter the name into the app, select the first occurence, back out of the menu, delete the name input and repeat until it stops or it breaks. This had a lot of issues as some climb names had punctuation chich means they would be re searched due to differnet saved names if I needed to rerun the script which I did have to do a few times.


## Future
- Extract all holds into some form of database 
- Use that to train new climbs
- Apply similar MoonBoard technique to validate climbs
- Create website for users to create climbs



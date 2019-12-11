'+-------------------+
'        SLAY
' ZEPPELIN GAMES 2019
'+-------------------+
$VERSIONINFO:CompanyName=Zeppelin Games
$VERSIONINFO:ProductName=Slay
$VERSIONINFO:LegalCopyright=(c) 2019 Zeppelin Games


_TITLE "SLAY" 'SET TITLE
_FULLSCREEN 'SET FULLSCREEN
SCREEN _NEWIMAGE(1200, 800, 32)

OPEN "SlayDebug.txt" FOR OUTPUT AS #1
CLOSE #1
Debug ("Game started")

'DECLARE VARIABLES
'SAVE DATA VARS
DIM SHARED save.point AS INTEGER
DIM SHARED saveData$(1000)

'PLAYER VARS
DIM SHARED player&
DIM SHARED playerJump&
DIM SHARED sword&
DIM SHARED player.name AS STRING
DIM SHARED player.health AS INTEGER
DIM SHARED player.basehealth AS INTEGER
DIM SHARED playerScale AS INTEGER
DIM SHARED swordScaleX AS INTEGER
DIM SHARED swordScaleY AS INTEGER
playerScale = 1
swordScaleX = 1
swordScaleY = 2

'ENEMY VARS
DIM SHARED dartImg&
DIM SHARED dartScale AS INTEGER
dartScale = 1

DIM SHARED pX

'PLAYER STATS
DIM SHARED stats.enemyskilled AS INTEGER
DIM SHARED stats.totaldeaths AS INTEGER

'SCREEN AND FORMATTING VARS
DIM SHARED w AS INTEGER
DIM SHARED h AS INTEGER

DIM SHARED fh AS INTEGER
DIM SHARED fw AS INTEGER

'GET THE SCREEN DIMSENSIONS
w = _WIDTH
h = _HEIGHT

'FONT DATA
fh = _FONTHEIGHT
fw = _FONTWIDTH

'SET THE COLOR
col = _RGBA(255, 255, 255, 255)
Debug ("Declared all variables")

'START GAME
Debug ("Loading Data")
LoadData

SUB LoadData
    'LOAD SAVE DATA

    'LOAD PLAYER SPRITE
    LoadSprite player&, playerScale, "PlayerSprite"
    LoadSprite playerJump&, playerScale, "PlayerJumpSprite"
    LoadSprite sword&, playerScale, "SwordSprite"
    LoadSprite dartImg&, dartScale, "DartSprite"
    Debug ("Loaded sprites")

    IF _FILEEXISTS("SlayV4SaveData.txt") THEN 'SEE IF THE FILE EXISTS
        OPEN "SlayV4SaveData.txt" FOR INPUT AS #1
        DO UNTIL EOF(1) 'LOOP UNTIL END OF FILE
            _LIMIT 60
            filecount = filecount + 1 'COUNT AMOUNT OF LINES
            LINE INPUT #1, file$
            saveData$(filecount) = file$
        LOOP
        CLOSE #1

        IF filecount > 2 THEN
            'PLAYER

            'DECRYPT NAME
            name$ = ""
            FOR n = 1 TO LEN(saveData$(1)) STEP 2
                name$ = name$ + CHR$(ASC(MID$(saveData$(1), n, 1)) - 4)
            NEXT n

            'DECRYPT STATS
            'HEALTH
            health$ = ""
            FOR n = 1 TO LEN(saveData$(2)) STEP 2
                health$ = health$ + CHR$(ASC(MID$(saveData$(2), n, 1)) - 4)
            NEXT n

            player.name = UCASE$(name$) 'LOAD NAME
            player.basehealth = VAL(health$) 'LOAD BASE HEALTH
            player.health = player.basehealth 'SET PLAYER HEALTH
            'SAVE DATA
            save.point = VAL(saveData$(3)) 'LOAD SAVE POINT
            'STATS
            stats.enemyskilled = VAL(saveData$(4)) 'ENEMYS KILLED
            stats.totaldeaths = VAL(saveData$(5)) 'TOTAL DEATHS
        ELSE
            Debug ("Creating new save")
            CreateNewSave
        END IF
    ELSE
        Debug ("Creating new save")
        CreateNewSave
    END IF

    Debug ("Loaded save data")
    'START GAME
    _DELAY (1)
    DrawLogo
END SUB

SUB CreateNewSave
    'CREATE NEW SAVE FILE
    scaledText w / 2 - 65, h / 2 + 8, 16, _RGBA(255, 255, 255, 255), "Enter your name: "
    DO
        _LIMIT 60
        LOCATE (h / 2 - 248) / fh + hf, (w / 2 - 50) / fw + fw: INPUT "", name$
    LOOP UNTIL LEN(name$) > 1
    IF LEN(name$) > 16 THEN
        CLS
        _PRINTSTRING (w / 2 - 150, h / 2), "Name length should be less than 16 characters"
        _DELAY (2)
        CLS
        LoadData
    END IF

    'SET VARS
    player.name = UCASE$(name$)
    player.health = 100

    save.point = 0
    stats.enemyskilled = 0
    stats.totaldeaths = 0

    'ENCRYPT SAVE DATA
    'NAME
    DIM nameArray$(16)
    FOR n = 1 TO LEN(player.name)
        nameArray$(n) = MID$(player.name, n, 1)
    NEXT n

    FOR n = 1 TO LEN(player.name)
        tempName$ = tempName$ + CHR$(ASC(nameArray$(n)) + 4)
        tempName$ = tempName$ + CHR$(INT(RND * 100) + 100)
    NEXT n

    name$ = tempName$

    'ENCRYPT STATS
    'HEALTH
    DIM healthArray$(5)
    FOR n = 1 TO LEN(STR$(player.health))
        healthArray$(n) = MID$(STR$(player.health), n, 1)
    NEXT n

    FOR n = 1 TO LEN(STR$(player.health))
        tempHealth$ = tempHealth$ + CHR$(ASC(healthArray$(n)) + 4)
        tempHealth$ = tempHealth$ + CHR$(INT(RND * 100) + 100)
    NEXT n

    'SAVE VARS TO TXT
    OPEN "SlayV4SaveData.txt" FOR OUTPUT AS #1
    'PLAYER
    PRINT #1, name$
    PRINT #1, tempHealth$
    'SAVE DATA
    PRINT #1, save.point
    'STATS
    PRINT #1, stats.enemyskilled
    PRINT #1, stats.totaldeaths
    CLOSE #1
    CLS
    Debug ("New save created")
END SUB

SUB DrawLogo
    'FADE LOGO IN
    FOR n = 0 TO 255
        'LIMIT FPS
        _LIMIT 60

        LINE (w / 10 * 2, h / 10 * 7)-(w / 10 * 8, h / 10 * 8), _RGB(0, 0, 0), BF
        scaledText w / 2, h / 2, 500, _RGBA32(255, 255, 255, n), "Z"
        scaledText w / 2, h / 6 * 4.5, 50, _RGBA(255, 255, 255, n), "ZEPPELIN GAMES"
        _DISPLAY
    NEXT n

    scaledText w / 2, h / 2, 500, _RGBA32(255, 255, 255, 255), "Z"
    scaledText w / 2, h / 6 * 4.5, 50, _RGBA32(255, 255, 255, 255), "ZEPPELIN GAMES"
    _DISPLAY
    _DELAY (3) 'WAIT A BIT

    'FADE THE LOGO AND TEXT OUT
    FOR n = 0 TO 255
        'LIMIT FPS
        _LIMIT 60

        LINE (w / 10 * 2, h / 10 * 7)-(w / 10 * 8, h / 10 * 8), _RGB(0, 0, 0), BF
        scaledText w / 2, h / 2, 500, _RGBA32(255, 255, 255, 255 - n), "Z"
        scaledText w / 2, h / 6 * 4.5, 50, _RGBA(255, 255, 255, 255 - n), "ZEPPELIN GAMES"
        _DISPLAY
    NEXT n
    CLS
    _DISPLAY
    DrawMenu
END SUB

SUB DrawMenu
    'FADE IN MENU SCREEN
    DIM col AS LONG
    col = _RGBA(255, 255, 255, 255)
    'S
    LINE (w / 10 * 2.75, h / 10 * 1.5)-(w / 10 * 3.5, h / 10 * 1.75), col, BF '1ST ACROSS
    LINE (w / 10 * 2.75, h / 10 * 1.5)-(w / 10 * 3, h / 10 * 2.5), col, BF '1ST DOWN
    LINE (w / 10 * 2.75, h / 10 * 2.5)-(w / 10 * 3.45, h / 10 * 2.75), col, BF '2ND ACROSS
    LINE (w / 10 * 3.2, h / 10 * 2.75)-(w / 10 * 3.45, h / 10 * 2.9), col, BF '2ND DOWN
    LINE (w / 10 * 3.2, h / 10 * 3.35)-(w / 10 * 3.45, h / 10 * 3.75), col, BF 'EXTENDED 2ND DOWN
    LINE (w / 10 * 2.65, h / 10 * 3.75)-(w / 10 * 3.45, h / 10 * 4.1), col, BF '3RD ACROSS

    'L
    LINE (w / 10 * 3.8, h / 10 * 1.5)-(w / 10 * 4.05, h / 10 * 2.65), col, BF '1ST DOWN
    LINE (w / 10 * 3.8, h / 10 * 3.6)-(w / 10 * 4.05, h / 10 * 3.75), col, BF 'EXTENDED 1ST DOWN
    LINE (w / 10 * 3.8, h / 10 * 3.75)-(w / 10 * 4.6, h / 10 * 4.1), col, BF '1ST ACROSS

    'A
    LINE (w / 10 * 4.85, h / 10 * 1.5)-(w / 10 * 5.1, h / 10 * 2.7), col, BF 'LEFT DOWN
    LINE (w / 10 * 4.85, h / 10 * 1.5)-(w / 10 * 5.75, h / 10 * 1.8), col, BF 'TOP LINE
    LINE (w / 10 * 5.5, h / 10 * 1.5)-(w / 10 * 5.75, h / 10 * 2.7), col, BF 'RIGHT DOWN
    LINE (w / 10 * 5.55, h / 10 * 1.5)-(w / 10 * 5.75, h / 10 * 2.9), col, BF 'RIGHT DOWN EDGE
    LINE (w / 10 * 4.85, h / 10 * 2.4)-(w / 10 * 5.75, h / 10 * 2.65), col, BF 'MIDDLE LINE
    LINE (w / 10 * 4.85, h / 10 * 3.5)-(w / 10 * 5.1, h / 10 * 4.1), col, BF 'EXTENDED LEFT DOWN
    LINE (w / 10 * 5.5, h / 10 * 3.5)-(w / 10 * 5.75, h / 10 * 4.1), col, BF 'EXTENDED RIGHT DOWN
    LINE (w / 10 * 5.55, h / 10 * 3.35)-(w / 10 * 5.75, h / 10 * 4.1), col, BF 'EXTENDED RIGHT DOWN EDGE

    'Y
    LINE (w / 10 * 6, h / 10 * 1.5)-(w / 10 * 6.25, h / 10 * 2.75), col, BF 'LEFT DOWN
    LINE (w / 10 * 6.75, h / 10 * 1.5)-(w / 10 * 7, h / 10 * 3), col, BF 'RIGHT DOWN
    LINE (w / 10 * 6, h / 10 * 2.5)-(w / 10 * 7, h / 10 * 2.85), col, BF '1ST ACROSS
    LINE (w / 10 * 6.75, h / 10 * 3.25)-(w / 10 * 7, h / 10 * 4.1), col, BF 'EXTENDED RIGHT DOWN
    LINE (w / 10 * 6, h / 10 * 3.8)-(w / 10 * 7, h / 10 * 4.1), col, BF 'BOTTOM LINE

    'SWORD
    LINE (w / 10 * 3, h / 10 * 3)-(w / 10 * 6.5, h / 10 * 3.25), col, BF 'HILT
    LINE (w / 10 * 3.5, h / 10 * 2.5)-(w / 10 * 3.75, h / 10 * 3.75), col, BF 'Hand Guard
    LINE (w / 10 * 3.75, h / 10 * 2.75)-(w / 10 * 4.75, h / 10 * 3.50), col, BF 'Blade First Layer
    LINE (w / 10 * 4.75, h / 10 * 2.85)-(w / 10 * 5.5, h / 10 * 3.4), col, BF 'Blade 2nd Layer
    LINE (w / 10 * 5.5, h / 10 * 3.10)-(w / 10 * 7, h / 10 * 3.15), col, BF 'Tip of Blade

    col = _RGBA(0, 0, 0, 255)
    LINE (w / 10, h / 10 * 4.5)-(w, h / 10 * 7), col, BF 'CLEAR OPTIONS

    'SHOW ACCOUNT
    _PRINTSTRING (5, 5), player.name

    'PRINT MENU OPTIONS
    scaledText w / 2, h / 10 * 4.75, 25, _RGBA(255, 255, 255, 255), "SINGLE-PLAYER"
    scaledText w / 2, h / 10 * 5.25, 25, _RGBA(255, 255, 255, 255), "MULTI-PLAYER"
    scaledText w / 2, h / 10 * 5.75, 25, _RGBA(255, 255, 255, 255), "OPTIONS"
    scaledText w / 2, h / 10 * 6.25, 25, _RGBA(255, 255, 255, 255), "QUIT"
    scaledText w / 2, h / 10 * 9.5, 14, _RGBA(255, 255, 255, 255), "TIP: USE 'w' AND 's' TO NAVIGATE. PRESS 'ENTER' TO SELCT"

    'PICK MENU OPTION
    opt = 0
    col = _RGBA(0, 0, 0, 255)
    DO
        _LIMIT 60
        LINE (w / 2 - 90, h / 10 * 4.5)-(w / 2 - 110, h / 10 * 7), col, BF 'CLEAR PAST POINTERS
        _PRINTSTRING (w / 2 - 100, h / 10 * (4.6 + (0.52 * opt))), ">" 'DISPLAY POINTER
        _DISPLAY 'REDUCE FLICKERING

        SELECT CASE INKEY$
            CASE "s", "S" 'MOVE POINTER DOWN
                IF opt + 1 = 4 THEN
                    opt = 0
                ELSE
                    opt = opt + 1
                END IF

            CASE "w", "W" 'MOVE POINTER UP
                IF opt - 1 = -1 THEN
                    opt = 3
                ELSE
                    opt = opt - 1
                END IF

            CASE CHR$(13) 'ENTER KEY
                'SINGLE-PLAYER SELECTED
                IF opt = 0 THEN
                    SinglePlayerMenu
                END IF

                'MULTI-PLAYER SELECTED
                IF opt = 1 THEN
                    MultiplayerGameMenu
                END IF

                'OPTIONS SELETED
                IF opt = 2 THEN
                    OptionsMenu
                END IF

                'QUIT SELECTED
                IF opt = 3 THEN
                    SYSTEM
                END IF
        END SELECT
    LOOP
END SUB


SUB OptionsMenu
    LINE (w / 10, h / 10 * 4.5)-(w, h / 10 * 7), col, BF 'CLEAR OPTIONS
    scaledText w / 2, h / 10 * 4.75, 25, _RGBA(255, 255, 255, 255), "INFO"
    scaledText w / 2, h / 10 * 5.25, 25, _RGBA(255, 255, 255, 255), "RESET GAME"
    scaledText w / 2, h / 10 * 5.75, 25, _RGBA(255, 255, 255, 255), "BACK"
    scaledText w / 2, h / 10 * 9.5, 14, _RGBA(255, 255, 255, 255), "TIP: USE 'w' AND 's' TO NAVIGATE. PRESS 'ENTER' TO SELCT"

    opt = 0
    DO
        _LIMIT 60
        LINE (w / 2 - 90, h / 10 * 4.5)-(w / 2 - 110, h / 10 * 7), col, BF 'CLEAR PAST POINTERS
        _PRINTSTRING (w / 2 - 100, h / 10 * (4.6 + (0.52 * opt))), ">" 'DISPLAY POINTER

        _DISPLAY 'REDUCE FLICKERING

        SELECT CASE INKEY$
            CASE "s", "S" 'MOVE POINTER DOWN
                IF opt + 1 = 3 THEN
                    opt = 0
                ELSE
                    opt = opt + 1
                END IF

            CASE "w", "W" 'MOVE POINTER UP
                IF opt - 1 = -1 THEN
                    opt = 2
                ELSE
                    opt = opt - 1
                END IF
            CASE CHR$(13) 'ENTER PRESSED
                IF opt = 0 THEN 'GAME INFO
                    LINE (0, h / 10 * 4.7)-(w, h), col, BF 'CLEAR OPTIONS
                    scaledText w / 2, h / 10 * 4.75, 25, _RGBA(255, 255, 255, 255), "ZEPPELIN GAMES 2018"
                    scaledText w / 2, h / 10 * 5.25, 25, _RGBA(255, 255, 255, 255), "SLAY V4"
                    scaledText w / 2, h / 10 * 6.25, 25, _RGBA(255, 255, 255, 255), "ENEMIES KILLED: " + STR$(stats.enemyskilled)
                    scaledText w / 2, h / 10 * 6.75, 25, _RGBA(255, 255, 255, 255), "TOTAL DEATHS: " + STR$(stats.totaldeaths)
                    scaledText w / 2, h / 10 * 9.25, 14, _RGBA(255, 255, 255, 255), "PRESS ANY KEY TO CONTINUE"

                    _DISPLAY
                    DO WHILE INKEY$ = ""
                    LOOP
                    LINE (0, h / 10 * 4.7)-(w, h), col, BF 'CLEAR OPTIONS
                    OptionsMenu
                END IF
                IF opt = 1 THEN 'RESET GAME
                    OPEN "SlayV4SaveData.txt" FOR OUTPUT AS #1
                    PRINT #1, ""
                    CLOSE #1
                    CLS
                    RUN "SLAY V4.exe"
                END IF
                IF opt = 2 THEN 'BACK
                    DrawMenu
                END IF
        END SELECT
    LOOP
END SUB

SUB SinglePlayerMenu
    col = _RGBA(0, 0, 0, 255)
    LINE (w / 10, h / 10 * 4.5)-(w, h / 10 * 7), col, BF 'CLEAR OPTIONS
    scaledText w / 2, h / 10 * 4.75, 25, _RGBA(255, 255, 255, 255), "CAMPAIGN"
    scaledText w / 2, h / 10 * 5.25, 25, _RGBA(255, 255, 255, 255), "INFINITE"
    scaledText w / 2, h / 10 * 5.75, 25, _RGBA(255, 255, 255, 255), "BACK"
    scaledText w / 2, h / 10 * 9.5, 14, _RGBA(255, 255, 255, 255), "TIP: USE 'w' AND 's' TO NAVIGATE. PRESS 'ENTER' TO SELCT"

    opt = 0
    DO
        _LIMIT 60
        LINE (w / 2 - 90, h / 10 * 4.5)-(w / 2 - 110, h / 10 * 7), col, BF 'CLEAR PAST POINTERS
        _PRINTSTRING (w / 2 - 100, h / 10 * (4.6 + (0.52 * opt))), ">" 'DISPLAY POINTER

        _DISPLAY 'REDUCE FLICKERING

        SELECT CASE INKEY$
            CASE "s", "S" 'MOVE POINTER DOWN
                IF opt + 1 = 3 THEN
                    opt = 0
                ELSE
                    opt = opt + 1
                END IF

            CASE "w", "W" 'MOVE POINTER UP
                IF opt - 1 = -1 THEN
                    opt = 2
                ELSE
                    opt = opt - 1
                END IF
            CASE CHR$(13) 'ENTER PRESSED
                IF opt = 0 THEN
                    Campaign
                END IF
                IF opt = 1 THEN
                    Infinite
                END IF
                IF opt = 2 THEN
                    DrawMenu
                END IF
        END SELECT
    LOOP
END SUB

'CAMPAIGN - STORY
SUB Campaign

END SUB

'PRACTICE - GAMEMODE
SUB Infinite
    CLS
    'DEFINE LOCAL VARS

    DIM health
    DIM deltaTime

    DIM swordX
    DIM swordY

    DIM swing
    DIM angle

    DIM facing

    TYPE dart
        image AS LONG
        x AS INTEGER
        y AS INTEGER
        alive AS INTEGER
    END TYPE

    DIM aliveEnemies AS INTEGER
    DIM enemySpeed AS INTEGER

    DIM darts(100) AS dart
    enemySpeed = 10

    FOR n = 1 TO 100
        darts(n).image = dartImg&
    NEXT n

    playerSpeed = 500
    jumpHeight = 500

    health = 100
    jump = 0
    pX = _WIDTH / 2 - (playerScale * 2.5)
    pY = _HEIGHT / 10 * 8

    swordX = pX
    swordY = pY + (playerScale * 5)

    sTime = TIMER

    DO
        _LIMIT 120
        sTime = TIMER(0.001)
        IF _KEYDOWN(100) THEN
            pX = pX + playerSpeed / 100
            facing = 1
            IF pX > _WIDTH - (playerScale * 12) - 1 THEN pX = _WIDTH - (playerScale * 12) - 1
        END IF
        IF _KEYDOWN(97) THEN
            pX = pX - playerSpeed / 100
            facing = -1
            IF pX < (playerScale * 12) THEN pX = (playerScale * 12)
        END IF
        IF _KEYDOWN(32) THEN
            IF jump = 0 THEN
                jump = 1
            END IF
        END IF

        IF NOT _KEYDOWN(32) THEN
            IF jump = 1 THEN
                jump = 2
            END IF
        END IF

        IF jump >= 1 THEN
            IF pY > ((_HEIGHT / 10) * 8) - jumpHeight AND NOT jump = 2 THEN
                pY = pY - playerSpeed / 100

            ELSE

                IF pY <= (_HEIGHT / 10) * 8 + 1 THEN
                    jump = 2
                    pY = pY + playerSpeed / 100
                ELSE
                    jump = 0
                    pY = (_HEIGHT / 10) * 8
                END IF
            END IF

            'Spin Sword
            swordX = pX + (playerScale * 125) * COS(_D2R(angle - 90))
            swordY = pY + (playerScale * 125) * SIN(_D2R(angle - 90))

        END IF

        IF jump = 1 THEN
            IF facing = 1 THEN
                angle = angle + ((_HEIGHT / 10 * 8) / ((_HEIGHT / 10 * 8) - pY) * 2)
            ELSE
                angle = angle - ((_HEIGHT / 10 * 8) / ((_HEIGHT / 10 * 8) - pY) * 2)
            END IF

        ELSE
            swordX = pX + (playerScale * 125) * COS(_D2R(-90))
            swordY = pY + (playerScale * 125) * SIN(_D2R(-90))
            angle = 0
        END IF


        CLS
        LINE (-1, (_HEIGHT / 10 * 8) + (playerScale * 25))-(_WIDTH + 1, _HEIGHT / 10 * 8.5), _RGB(255, 255, 255), BF

        IF jump >= 1 THEN
            RotoZoom2 pX, pY, playerJump&, playerScale, playerScale, 0
        ELSE

            RotoZoom2 pX, pY, player&, playerScale, playerScale, 0
        END IF


        FOR n = 1 TO 100
            IF darts(n).alive = 1 THEN
                darts(n).x = darts(n).x + (pX - darts(n).x / deltaTime) * enemySpeed
                darts(n).y = darts(n).y + (pY - darts(n).y / deltaTime) * enemySpeed
            END IF

            col = CheckCollision(darts(n).image, sword&, darts(n).x, darts(n).y, swordX, swordY)
            IF col = 1 THEN
                darts(n).alive = 0
                aliveEnemies = aliveEnemies - 1
            END IF

            col = CheckCollision(darts(n).image, player&, darts(n).x, darts(n).y, pX, pY)
            IF col = 1 THEN
                darts(n).alive = 0
                PRINT "GAMEOVER"
            END IF

        NEXT n

        RotoZoom2 swordX, swordY, sword&, swordScaleX, swordScaleY, angle + 180
        _DISPLAY

        eTime = TIMER(0.001)
        deltaTime = eTime - sTime

    LOOP UNTIL health <= 0 OR INKEY$ = CHR$(27)
    CLS
    SaveGame
    DrawMenu
END SUB

'MULTIPLAYER GAME MENU
SUB MultiplayerGameMenu
    LINE (w / 10, h / 10 * 4.7)-(w, h / 10 * 7), col, BF 'CLEAR OPTIONS
    _PRINTSTRING (w / 2 - 40, h / 10 * 4.75), "LOCAL"
    _PRINTSTRING (w / 2 - 34, h / 10 * 5.25), "LAN"
    _PRINTSTRING (w / 2 - 37, h / 10 * 5.75), "BACK"

    opt = 0
    DO
        LINE (w / 2 - 58, h / 10 * 4.7)-(w / 2 - 70, h / 10 * 7), col, BF 'CLEAR PAST POINTERS
        _PRINTSTRING (w / 2 - 65, h / 10 * (4.75 + (0.5 * opt))), ">" 'DISPLAY POINTER

        _DISPLAY 'REDUCE FLICKERING

        SELECT CASE INKEY$
            CASE "s" 'MOVE POINTER DOWN
                IF opt + 1 = 3 THEN
                    opt = 0
                ELSE
                    opt = opt + 1
                END IF

            CASE "w" 'MOVE POINTER UP
                IF opt - 1 = -1 THEN
                    opt = 2
                ELSE
                    opt = opt - 1
                END IF
            CASE CHR$(13) 'ENTER KEY
                IF opt = 0 THEN
                    LocalMultiplayer
                ELSEIF opt = 1 THEN
                    LANMultiplayer
                ELSEIF opt = 2 THEN
                    DrawMenu
                END IF
        END SELECT
    LOOP UNTIL INKEY$ = CHR$(13)
END SUB


'MULTIPLAYER ON SAME DEVICE
SUB LocalMultiplayer

END SUB

'MULTIPLAYER OVER WIFI CONNECTION
SUB LANMultiplayer


END SUB

SUB SaveGame
    'SAVE VARS TO TXT
    OPEN "SlayV4SaveData.txt" FOR OUTPUT AS #1
    'PLAYER
    PRINT #1, name$
    PRINT #1, player.health
    'SAVE DATA
    PRINT #1, save.point
    'STATS
    PRINT #1, stats.enemyskilled
    PRINT #1, stats.totaldeaths
    CLOSE #1
END SUB

SUB LoadSprite (sp&, scale, name$)
    Debug "Loading sprite " + name$
    DIM sprite(4, 4)
    FOR y = 0 TO 4
        FOR x = 0 TO 4
            READ sprite(x, y) 'Read the data
        NEXT x
    NEXT y

    FOR y = 0 TO 4 * scale STEP scale 'Factor in the scale
        FOR x = 0 TO 4 * scale STEP scale
            spriteCol = sprite(x / scale, y / scale)
            LINE (x, y)-(x + scale, y + scale), _RGB32(spriteCol * 255, spriteCol * 255, spriteCol * 255), BF 'Draw each pixel
        NEXT x
    NEXT y
    sp& = _NEWIMAGE(50, 50, 32)
    _PUTIMAGE , 0, sp&, (0, 0)-(scale * 4, scale * 4)
    CLS
END SUB

SUB scaledText (x, y, textHeight, K AS _UNSIGNED LONG, txt$)
    fg = _DEFAULTCOLOR
    'screen snapshot
    cur& = _DEST
    I& = _NEWIMAGE(8 * LEN(txt$), 16, 32)
    _DEST I&
    COLOR K, _RGBA32(0, 0, 0, 0)
    _PRINTSTRING (0, 0), txt$
    mult = textHeight / 16
    xlen = LEN(txt$) * 8 * mult
    _PUTIMAGE (x - .5 * xlen, y - .5 * textHeight)-STEP(xlen, textHeight), I&, cur&
    COLOR fg
    _FREEIMAGE I&
END SUB

SUB RotoZoom2 (X AS LONG, Y AS LONG, Image AS LONG, xScale AS SINGLE, yScale, Rotation AS SINGLE)
    DIM px(3) AS SINGLE: DIM py(3) AS SINGLE
    W& = _WIDTH(Image&): H& = _HEIGHT(Image&)
    px(0) = -W& / 2: py(0) = -H& / 2: px(1) = -W& / 2: py(1) = H& / 2
    px(2) = W& / 2: py(2) = H& / 2: px(3) = W& / 2: py(3) = -H& / 2
    sinr! = SIN(-Rotation / 57.2957795131): cosr! = COS(-Rotation / 57.2957795131)
    FOR i& = 0 TO 3
        x2& = (px(i&) * cosr! + sinr! * py(i&)) * xScale + X: y2& = (py(i&) * cosr! - px(i&) * sinr!) * yScale + Y
        px(i&) = x2&: py(i&) = y2&
    NEXT
    _MAPTRIANGLE (0, 0)-(0, H& - 1)-(W& - 1, H& - 1), Image& TO(px(0), py(0))-(px(1), py(1))-(px(2), py(2))
    _MAPTRIANGLE (0, 0)-(W& - 1, 0)-(W& - 1, H& - 1), Image& TO(px(0), py(0))-(px(3), py(3))-(px(2), py(2))
END SUB

SUB Debug (debuglog$)
    OPEN "SlayDebug.txt" FOR APPEND AS #1
    PRINT #1, DATE$ + " " + TIME$ + ": " + debuglog$
    CLOSE #1
END SUB


FUNCTION CheckCollision (c1 AS LONG, c2 AS LONG, c1x, c1y, c2x, c2y)
    IF c1x < c2x + _WIDTH(c2) AND c1x + _WIDTH(c1) > c2x THEN
        IF c1y < c2y + _HEIGHT(c2) AND c1y + _HEIGHT(c1) > c2y THEN
            CheckCollision = 1
        ELSE
            CheckCollision = 0
        END IF
    ELSE
        CheckCollision = 0
    END IF
END FUNCTION


'Player Data
DATA 0,1,1,1,0
DATA 0,1,1,1,0
DATA 1,1,1,1,1
DATA 0,1,1,1,0
DATA 0,1,0,1,0

'Player Jump Data
DATA 0,1,1,1,0
DATA 0,1,1,1,0
DATA 1,1,1,1,1
DATA 0,1,1,1,0
DATA 1,1,1,1,1

'Sword Data
DATA 0,0,1,0,0
DATA 1,1,1,1,1
DATA 0,1,1,1,0
DATA 0,1,1,1,0
DATA 0,0,1,0,0

'Dart 1 Data
DATA 1,0,0,0,0
DATA 1,1,1,1,0
DATA 1,1,1,1,1
DATA 1,1,1,1,0
DATA 1,0,0,0,0


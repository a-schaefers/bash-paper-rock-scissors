```bash`
#!/bin/bash

# deps: bash, shuf, grep, cut, fontawesome

main () {
    play () {
        [ "$1" = d ] && { echo DYNAMITE IS CHEATING!; echo EXITING PROGRAM! CHEATER!; exit 1; }

        game=(rock paper scissors)

        rules=(" NPC:  YOU:  RESULT: ROCK = ROCK, YOU TIED!"
               " NPC:  YOU:  RESULT: PAPER > ROCK, YOU WIN!"
 " NPC:  YOU:  RESULT: ROCK > PAPER, YOU LOSE!"
               " NPC:  YOU:  RESULT: PAPER > ROCK, YOU LOSE!"
               " NPC:  YOU:  RESULT: PAPER = PAPER, YOU TIED!"
               " NPC:  YOU:  RESULT: SCISSORS > PAPER, YOU WIN!"
               " NPC:  YOU:  RESULT: ROCK > SCISSORS, YOU WIN!"
               " NPC:  YOU:  RESULT: SCISSORS > PAPER, YOU LOSE!"
               " NPC:  YOU:  RESULT: SCISSORS = SCISSORS, YOU TIED!")

        for x in "${game[@]}"; do
            for y in "${game[@]}"; do
                combinations+=("$x$y-${rules[0]}")
                rules=("${rules[@]:1}")
            done
        done

        npc="$(echo -e "rock\npaper\nscissors" | shuf -n 1;)"
        printf "%s \n" "${combinations[@]}" | grep "$npc$1" | cut -d '-' -f 2
        combinations=() ; game=() ; rules=()

        return 0
    }

    while :; do
        read -p "(r)ock, (p)aper, (s)cissors? ---- (q)uit " CHOICE
        case "$CHOICE" in
            r|p|s|d) play $CHOICE ;;
            q) exit 0 ;;
            *) echo "Invalid option: $CHOICE" ; continue ;;
        esac
    done
}

main
````

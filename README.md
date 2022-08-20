# Using nested for loops creates a grid

e.g.

```bash
list=(x y z)

for x in ${list[@]}; do for y in ${list[@]}; do echo "$x, $y"; done; done

x, x
x, y
x, z
y, x
y, y
y, z
z, x
z, y
z, z`
````

In the game code below

First declare the rules to the game (rules)

Then use the nested for loops to create all the possible options (combinations)

Then Generate a random computer choice

Finally take an input for a user choice and compare the two against the rules

It all turns in to something like as follows, without any tedious if,then,else logic - but hardly readable in bash:

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

It turns out after doing all of this, someone on the internet told me the game could be summarized in only one line of code,

```bash
echo -e "rock\npaper\nscissors" | shuf -n 1
```

PR accepted if someone wants to add a netcat http server and use Bash's ability to tap in to `/dev/tcp/host/port` for multiplayer support.

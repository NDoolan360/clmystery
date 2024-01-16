# My solution

## Clues

```bash
grep CLUE *

# crimescene:CLUE: Footage from an ATM security camera is blurry but shows that the perpetrator is a tall male, at least 6'.
# crimescene:CLUE: Found a wallet believed to belong to the killer: no ID, just loose change, and membership cards for AAA, Delta SkyMiles, the local library, and the Museum of Bash History. The cards are totally untraceable and have no name, for some reason.
# crimescene:CLUE: Questioned the barista at the local coffee shop. He said a woman left right before they heard the shots. The name on her latte was Annabel, she had blond spiky hair and a New Zealand accent.
```

## Found Annabel

```bash
grep Annabel people | grep \\sF\\s

# Annabel Sun     F       26      Hart Place, line 40
# Annabel Church  F       38      Buckingham Place, line 179
```

### Interviewed Annabel's

```bash
head -n 40 ./streets/Hart_Place | tail -1

# SEE INTERVIEW #47246024
```

```bash
head -n 179 ./streets/Buckingham_Place | tail -1

# SEE INTERVIEW #699607
```

### Annabel Church Interview

```bash
cat interviews/interview-699607

# Interviewed Ms. Church at 2:04 pm.  Witness stated that she did not see anyone she could identify as the shooter, that she ran away as soon as the shots were fired.

# However, she reports seeing the car that fled the scene.  Describes it as a blue Honda, with a license plate that starts with "L337" and ends with "9"
```

## Finding the perp

### Memberships

```bash
grep -f ./memberships/Delta_SkyMiles ./memberships/AAA | grep -f ./memberships/Terminal_City_Library | grep -f ./memberships/Museum_of_Bash_History
```

### Car

```bash
grep -A 5 "L337..9" vehicles | grep -A 3 Honda | grep -A 2 Blue
```

### Height

```bash
grep -B 1 -E "(5'1[0-2])|(6'[0-2])" vehicles
```

### Gender

```bash
grep \\sM\\s people | cut -d"      " -f 1
```

## Altogether

```bash
grep -f ./memberships/Delta_SkyMiles ./memberships/AAA | grep -f ./memberships/Terminal_City_Library | grep -f ./memberships/Museum_of_Bash_History |
grep -B3 -A2 -f- vehicles | grep -A 5 "L337..9" | grep -A 3 Honda | grep -A 2 Blue |
grep -B 1 -E "(5'1[0-2])|(6'[0-2])" | grep Owner | cut -d" " -f1 --comp |
grep -f- people | grep \\sM\\s

# NAME            GENDER  AGE     ADDRESS
# Jeremy Bowers   M       34      Dunstable Road, line 284
```


# mic
I use Skype, Linphone and Hangouts every day at work and I constantly forget to mute the damn mic whenever I'm on a call but not participating on it actively or just waiting for my turn to speak.
I created this little blocklet that hopefully will help me be aware whenever my mic is open so I can mute it and keep things professionally :)

## Screenshots

![micc_alt](https://cloud.githubusercontent.com/assets/394089/6995546/6bddf82c-db25-11e4-8081-96101f4a7921.png)
![mico](https://cloud.githubusercontent.com/assets/394089/6995547/6be11034-db25-11e4-9b59-bdfc06f1181f.png)
![mico_alt](https://cloud.githubusercontent.com/assets/394089/6995548/6be340e8-db25-11e4-9379-075b8aad51eb.png)
![micc](https://cloud.githubusercontent.com/assets/394089/6995549/6be567b0-db25-11e4-85be-3ccbd91b22ce.png)

I already ship two variants of the mics shown - also the colors can be customized too.

**Fonts utilized:**
i3: _font pango:Terminus 9_  (frames, dialog boxes, status bar)
Eye candy: _Font Awesome_


## Code
Relevant **~/.config/i3blocks/config** stuff:
```bash
...
command=~/.config/i3blocks/blocklets/$BLOCK_NAME
[mic]
color=#FFFFFF
interval=5
...
```

**~/.config/i3blocks/blocklets/mic**:
```bash
#!/bin/bash
# Copyright (C) 2015 Martín Cigorraga <archlinux.us: msx>

# This program is free software: you can redistribute it and/or modify
# it under the terms of the Affero GNU General Public License as published
# by the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.

# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.

# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.


# Check mouse event
case $BLOCK_BUTTON in
    3)
	#/usr/bin/amixer set Capture toggle &>/dev/null;  # AlsaMixer
	/usr/bin/pactl set-source-mute 1 toggle;  # PulseAudio
esac


# Determine mic's state
#amixer contents | grep -q "values=off" && [[ $? -eq 0 ]] && state="off";  # If you use AlaMixer
pactl list sources | grep -q "Mute: yes" && [[ $? -eq 0 ]] && state="off";  # If you rather go with PulseAudio


# Toggle mic's state
case $state in
    off)
	echo "  " " muted ";
	#echo "  "
	echo
	echo \#005FD7;  # Soft blue
	#echo \#00FF00;  # Bright green
	exit 0;
	;;
    *)
	echo "     " " OPEN   ";
	#echo "      ";
	exit 33;
	;;
esac

exit 0;
```

### Features
* Avoids possibly mistakes when you are unaware the mic is open :+1: 
* Lets you toggle MUTE state with a right-click.
* The script reads mic's state every 5 seconds (default) so if you set the mic state using AlsaMixer or similar app the change will be reflected on next refresh.
* I ship the script ready to use it with AlsaMixer and PulseAudio (default), you can toggle which backend you prefer by simply uncommenting the appropriate lines.
* If you happen to sometimes be faraway as I am, you will probably choose to use the bigger alert. However as everything is in plain text you can customize anything to your liking.

### Requirements
###### For toggling mic's state:
* _**amixer**_ (AlsaMixer)
* _**pactl**_ (PulseAudio)

###### Eye candy:
* _**Font Awesome**_, get it here: https://fortawesome.github.io/Font-Awesome/cheatsheet/
* _**Terminus**_ font (probably already shipped in your distro's repositories)

That's all for now, I hope **mic** will be useful for someone else.
_Any ideas/critics on how to improve this circus is welcome (in fact I would love to know about them)_.

_by [msx](https://github.com/msx)_

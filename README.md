install i2p on a debian live dvd r like tails ...





Yes, Debian Live + i2pd can run from a DVD-R, but there are some limitations.
How it works
You can create:
DVD-R → Debian Live → i2pd → I2P network
The computer boots from the DVD, runs Debian in RAM, and i2pd runs during that session.
The good points
✅ No internal disk needed
✅ No persistence (everything disappears after reboot)
✅ Can run offline/air-gapped except the network connection you allow
✅ More flexible than Tails because you can install normal Debian packages
The limitations
❌ A normal Debian Live DVD is read-only
You cannot permanently install i2pd on the DVD.
Any package installation disappears after shutdown.



Option 1 
— Install i2pd every boot

Boot Debian Live, then:
sudo apt update
sudo apt install i2pd
sudo systemctl start i2pd
It works, but:
slower
you repeat it every session
requires internet access to download packages

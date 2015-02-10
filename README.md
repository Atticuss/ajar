ajar.py
=======

```ajar.py -b <backdoor.java> -t <target.jar> [-o <outfile.jar>]```

Injects backdoor.java into target.jar. May prove useful in MitM attacks. This happens by:
  1. Unzipping target.jar to ./tmp/
  2. Parsing manifest for Main-Class value
  3. Changing Main-Class to the backdoor
  4. Modifying the backdoor to call the main() method of the original initial .class at the end of it's own main() method
  5. Compiling the modified backdoor.java to ./tmp/backdoor.class
  6. Rezipping ./tmp/* to outfile.jar (default is backdoor.jar)

If the backdoor is a persistent function (such as a shell listening on a socket), make sure to spin it off into it's own thread or the main app will never execute. Tool assumes javac is in your path and you can read/write/create to a tmp dir in the running location. Some .jar files don't seem to play nicely when unzipping. Notable example of this is the free version of Burp1.6 (although the pro version worked fine).

##Future Improvements
Looking into directly modifying Java Bytecode for inserting backdoor. This should reduce overall runtime. Unzipping .jar is a major timesink.

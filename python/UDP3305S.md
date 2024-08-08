# UPD3305S

Python class for controlling Uni-T UDP3305S or UDP3305S-E lab power supply units.

# Example script

    #!/bin/env python3
    import time, datetime
    from UDP3305S import UDP3305S

    psu = UDP3305S("TCPIP::192.168.0.66::INSTR")

    # setup voltage and current limits
    psu.ch1.set_voltage(13.5)
    psu.ch1.set_current(3)
    psu.ch2.set_voltage(24)
    psu.ch2.set_current(4.5)

    # activate the first two channels
    psu.ch1.on()
    psu.ch2.on()

    # log current and power for about 30 seconds
    print("timestamp, A1, P1, A2, P2")
    for i in range(30):
        print(", ".join([str(x) for x in [
                datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
                psu.ch1.read_current(),
                psu.ch1.read_power(),
                psu.ch1.read_current(),
                psu.ch1.read_power(),
        ]]))
        time.sleep(1)

    # turn off all channels
    psu.off()


# Reference

You can get this from pydoc anytime.

    class UDP3305S(builtins.object)
     |  UDP3305S(RID)
     |  
     |  Uni-T UDP3305S Lab power supply
     |  
     |  Features 5 channels:
     |  
     |      ch1     channel1
     |      ch2     channel2
     |      ch3     channel3
     |      chSER   virtual channel vor serial mode
     |      chPAR   virtual channel for parallel mode
     |  
     |  Methods defined here:
     |  
     |  __del__(self)
     |  
     |  __init__(self, RID)
     |      Initialize PSU instance
     |      
     |      RID (Resource ID) as defined by pyVISA. E.g.:
     |      
     |          TCPIP::192.168.0.66::INSTR
     |          TCPIP::PowerSupply::INSTR
     |          GPIB1::10
     |          USB::0x1234::125::A22-5::INSTR
     |      
     |          See pyVISA documentation for details.
     |  
     |  __str__(self)
     |      Return str(self).
     |  
     |  get_mode(self)
     |      return output mode (NORMAL | SER | PARA)
     |  
     |  lock(self)
     |      lock keys on instrument panel
     |  
     |  off(self)
     |      turn off all outputs
     |  
     |  on(self)
     |      turn on all outputs
     |  
     |  set_mode(self, mode)
     |      set output mode (NORMAL | SER | PARA)
     |  
     |  unlock(self)
     |      unlock keys on instrument panel
     |  
     |  ----------------------------------------------------------------------
     |  Data descriptors defined here:
     |  
     |  __dict__
     |      dictionary for instance variables (if defined)
     |  
     |  __weakref__
     |      list of weak references to the object (if defined)
    
    class channel(builtins.object)
     |  channel(name, connection, V_max, A_max)
     |  
     |  PSU channel
     |  Implementaton of all channel features.
     |  
     |  Methods defined here:
     |  
     |  __init__(self, name, connection, V_max, A_max)
     |      Initialize channel object
     |      
     |      name        name of the channel
     |      connection  connection object to read/write from/to
     |      V_max       max voltage supported
     |      A_max       max current supported
     |  
     |  get_OCP(self)
     |      return over current protection (OCP) value [A]
     |  
     |  get_OVP(self)
     |      return over voltage protection (OVP) value [V]
     |  
     |  get_current(self)
     |      get current limit [A]
     |  
     |  get_voltage(self)
     |      get output voltage [V]
     |  
     |  off(self)
     |      turn output off
     |  
     |  on(self)
     |      turn output on
     |  
     |  read_all(self)
     |      read (measure) output values: Volts [V], current [A], Power[W]
     |  
     |  read_current(self)
     |      read (measure) output current [A]
     |  
     |  read_power(self)
     |      read (measure) output power [W]
     |  
     |  read_voltage(self)
     |      read (measure) output voltage [V]
     |  
     |  set_OCP(self, value, state=1)
     |      set over current protection (OCP) value [A]
     |  
     |  set_OVP(self, value, state=1)
     |      set over voltage protection (OVP) value [V]
     |  
     |  set_current(self, value)
     |      set current limit [A]
     |  
     |  set_voltage(self, value)
     |      set output voltage [V]




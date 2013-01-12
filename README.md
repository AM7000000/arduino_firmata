arduino_firmata
===============

* Arduino Firmata protocol (http://firmata.org) implementation on Ruby.
* http://shokai.github.com/arduino_firmata


Install
-------

    % gem install arduino_firmata


Requirements
------------

* Arduino Standard Firmata v2.2
  * Arduino IDE -> [File] -> [Examples] -> [Firmata] -> [StandardFirmata]
* Ruby 1.8.7+
* Ruby 1.9.2+


Synopsis
--------

Setup
```ruby
require 'arduino_firmata'

arduino = ArduinoFirmata.connect  # use default arduino
arduino = ArduinoFirmata.connect '/dev/tty.usb-device-name'
arduino = ArduinoFirmata.connect '/dev/tty.usb-device-name', :bps => 57600
```

Board Version
```ruby
puts "firmata version #{arduino.version}"
```

Digital Write
```ruby
arduino.digital_write 13, true
arduino.digital_write 13, false
```

Digital Read
```ruby
arduino.pin_mode 7, ArduinoFirmata::INPUT
puts arduino.digital_read 7  # => true/false
```

Analog Write (PWM)
```ruby
0.upto(255) do |i|
  arduino.analog_write 11, i
  sleep 0.01
end
```

Analog Read
```ruby
puts arduino.analog_read 0  # => 0 ~ 1023

arduino.on :analog_read 0 do |pin, value|
  if pin == 0
    puts "analog pin #{pin} changed #{value}"
  end
end
```

Servo Motor
```ruby
loop do
  angle = rand(180)
  arduino.servo_write 11, angle
  sleep 1
end
```

Close
```ruby
arduino.close
```

Block
```ruby
ArduinoFirmata.connect do
  puts "firmata version #{version}"

  30.times do
    an = analog_read 0
    analog_write 11, an
    sleep 0.01
  end
end
```

Send Sysex Command
```ruby
arduino.sysex 0x01, [13, 5, 2]
```

Regist Sysex Command Event
```ruby
arduino.on :sysex do |command, data|
  puts "command : #{command}"
  puts "data    : #{data.inspect}"
end
```

see samples https://github.com/shokai/arduino_firmata/tree/master/samples


Test
----

    % gem install bundler
    % bundle install
    % export ARDUINO=/dev/tty.usb-device-name
    % rake test


Contributing
------------
1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
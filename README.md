# Thinkpad 700C Power Supply Board

I've reverse engineered the power board from a Thinkpad 700C. The P/N is 35G4785, and the FRU number is 48G3712. A very similar or identical board is also present in the Thinkpad 700.

The schematic isn't perfect. Open a bug report if you find a mistake. Not all components were identified. A number of parts did not have designators on the silkscreen, so I assigned them new ones starting at 300.

The circuit board has four layers and an aluminum core, presumably for heat dissipation.

**Do not fabricate this design. The PCB layout is for reference only.** There are ground plane cuts in layer User.9 that need to get transferred to the ground plane. There are minor footprint differences as well. There are still unidentified components in the bill of materials.

## Disassembly

If you need information about disassembling a Thinkpad 700C to access the power board, refer to [my guide on iFixit](https://www.ifixit.com/Guide/IBM+Thinkpad+700C+Disassembly/173038).

## Troubleshooting

There is a single aluminum electrolytic capacitor on this board, C18 (47uF 6.3V) and, at least in my machine, it failed and leaked electrolyte everywhere. This is a problem because the electrolyte likes to puddle in nearby vias, and the acid in the solution etches away and can break the connection.

C43 and C19 are polymer electrolytic capacitors and are much more reliable than C18.

You can use the board layout to help with troubleshooting since it shows how the vias in this area should be connected.

## Operation

This is a very complicated design. I've added some notes to the schematic to hopefully make it easier to understand. Basically there is an input protection, power ORing circuit, and a set of four DC-DC converters. Two of them produce the main 5V rail--one of them from the tiny standby battery, the other from the battery+AC adapter. The remaining two generate two additional voltages. Several additional power rails are derived from these three but are gated by series-pass transistors, presumably for handling all the various power saving modes.

There is voltage monitoring for both overvoltage and undervoltage. Overvoltage causes a crowbar circuit to slam the AC adapter+battery voltage to zero. Undervoltage causes a power good output to deassert.

A pretty complicated circuit handles the power turn on and turn off, with many control lines coming from the motherboard.

Resistors with the "VR" prefix are laser-trimmed resistors on the circuit board itself. Using some sort of test fixture, the factory would measure critical voltage using a series of test points and make adjustments by firing a laser at these resistors.

# IBM Thinkpad Power Adapter 48G8942

I've also reverse engineered a compatible power adapter, FRU 48G8942. This is designed to output 10 to 20V at 2.20A or 3.38A.

## Design

The power adapter is a relatively straightforward design. IC051 is a primary-side switching power supply controller that produces 20VDC on the secondary side output. IC151 is a dual op-amp chip that regulates either voltage or current, as determined by D151. If the current is below the regulation limit, then the voltage control loop regulates the output voltage to 20V. If the current limit is reached, then the power supply lowers the voltage to limit the current.

There are two possible values for the current regulation, controlled by the state of the "INPUT SIGNAL" pin on the power plug.

* Floating or high: 2.2A
* Pulled low: 3.38A

The Thinkpad laptop pulls this pin low when it is operating, and allows it to float when it is powered off or in standby mode.

Unlike modern laptops, this early Thinkpad power supply also acts as the battery charger, so it tries to charge the batteries with a constant current. If the laptop is operating, it is also using current, so the charger has to increase the current to compensate and maintain the same charging current.

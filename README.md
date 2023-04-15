# Thinkpad 700C Power Supply Board

I've reverse engineered the power board from a Thinkpad 700C. The P/N is 35G4785, and the FRU number is 48G3712. A very similar or identical board is also present in the Thinkpad 700.

The schematic isn't perfect. Open a bug report if you find a mistake. Not all components were identified. A number of parts did not have designators on the silkscreen, so I assigned them new ones starting at 300.

The circuit board has four layers and an aluminum core, presumably for heat dissipation.

# Troubleshooting

There is a single aluminum electrolytic capacitor on this board, C18 (47uF 6.3V) and, at least in my machine, it failed and leaked electrolyte everywhere. This is a problem because the electrolyte likes to puddle in nearby vias, and the acid in the solution etches away and can break the connection.

C43 and C19 are polymer electrolytic capacitors and are much more reliable than C18.

# Operation

This is a very complicated design. I've added some notes to the schematic to hopefully make it easier to understand. Basically there is an input protection, power ORing circuit, and a set of four DC-DC converters. Two of them produce the main 5V rail--one of them from the tiny standby battery, the other from the battery+AC adapter. The remaining two generate two additional voltages. Several additional power rails are derived from these three but are gated by series-pass transistors, presumably for handling all the various power saving modes.

There is voltage monitoring for both overvoltage and undervoltage. Overvoltage causes a crowbar circuit to slam the AC adapter+battery voltage to zero. Undervoltage causes a power good output to deassert.

A pretty complicated circuit handles the power turn on and turn off, with many control lines coming from the motherboard.

Resistors with the "VR" prefix are laser-trimmed resistors on the circuit board itself. Using some sort of test fixture, the factory would measure critical voltage using a series of test points and make adjustments by firing a laser at these resistors.

### Setup patterns

A controller is outside of the ruby network.
To be connected to the ruby network, the controller must be connected to a ruby router via a ruby external link.
The same ruby router must be connected to another router in the network via some internal link(s).
This holds for all controllers.

### Debugging

- GDB should help a lot!

### Frequent Issues

#### Problem 1

```
build/ARM_MESI_Three_Level/mem/ruby/system/RubySystem.cc:151: fatal: fatal condition !machineToNetwork.count(mach_id) occurred: No machineID DMA_1. Does not belong to a Ruby network?
```

This usually means the controller is not connected to a ruby external link, or the external link somehow is not part of the ruby network.

#### Problem 2
```
gem5.debug: build/ARM_MESI_Three_Level/mem/ruby/network/Topology.cc:223: void gem5::ruby::Topology::addLink(gem5::ruby::SwitchID, gem5::ruby::SwitchID, gem5::ruby::BasicLink*, gem5::ruby::PortDirection, gem5::ruby::PortDirection): Assertion `dest <= m_number_of_switches+m_nodes+m_nodes' failed.
```

The ruby network keeps track of all int_links, ext_links, and routers via `ruby_system.network.int_links`, `ruby_system.network.ext_links`, and `ruby_system.network.routers` respectively.
The above error usually indicates some of the links and routers were not added to the above variables.

### CHI

#### Finding where request/response/data messages are generated

Find,

- `enqueue\([a-zA-Z]*, CHIRequestMsg,`
- `enqueue\([a-zA-Z]*, CHIResponseMsg,`
- `enqueue\([a-zA-Z]*, CHIDataMsg,`

#### Structures

- `Mem` and `Cache` have different `TBE` structures, but share the same name in the slicc files.

#### Debug Flags

- `ProtocolTrace`: printing actions and state transitions.
- `RubyGenerated`: which functions are called per action.


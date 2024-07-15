# bit_ops

Common bit-oriented operations on primitive integer types with a focus on
`no_std` and `const` compatibility. Unlike other crates that provide tooling to
create sophisticated high-level types with bitfields, the focus of `bit_ops` is
on raw primitive integer types.

# Documentation

See <https://docs.rs/bit_ops>.

# Example

<!-- copied from lib.rs -->
```rust
fn main() {
    // PREREQUISITES: Some Definitions

    /// See specification of the x86 IOAPIC redirection entry for more details.
    mod x86_ioapic {
        pub const VECTOR_BITS: u64 = 8;
        pub const VECTOR_SHIFT: u64 = 0;
        pub const DELIVERY_MODE_BITS: u64 = 3;
        pub const DELIVERY_MODE_SHIFT: u64 = 8;
        pub const DESTINATION_MODE_BITS: u64 = 1;
        pub const DESTINATION_MODE_SHIFT: u64 = 11;
        pub const PIN_POLARITY_BITS: u64 = 1;
        pub const PIN_POLARITY_SHIFT: u64 = 13;
        pub const TRIGGER_MODE_BITS: u64 = 1;
        pub const TRIGGER_MODE_SHIFT: u64 = 15;
        pub const MASKED_BITS: u64 = 1;
        pub const MASKED_SHIFT: u64 = 16;
        pub const DESTINATION_BITS: u64 = 8;
        pub const DESTINATION_SHIFT: u64 = 56;
    }

    use x86_ioapic::*;

    // ACTUAL LIBRARY USAGE BEGINS HERE

    let redirection_entry = bit_ops::bitops_u64::set_bits_exact_n(
        0,
        &[
            (7, VECTOR_BITS, VECTOR_SHIFT),
            (0b111 /* ExtInt */, DELIVERY_MODE_BITS, DELIVERY_MODE_SHIFT),
            (0 /* physical */, DESTINATION_MODE_BITS, DESTINATION_MODE_SHIFT),
            (1 /* low-active */, PIN_POLARITY_BITS, PIN_POLARITY_SHIFT),
            (1 /* level-triggered */, TRIGGER_MODE_BITS, TRIGGER_MODE_SHIFT),
            (1 /* masked */, MASKED_BITS, MASKED_SHIFT),
            (13 /* APIC ID */, DESTINATION_BITS, DESTINATION_SHIFT),
        ],
    );
    assert_eq!(redirection_entry, 0xd0000000001a707);
}
```

## MSRV

1.57.0 stable

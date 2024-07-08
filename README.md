# RapidTime [![RapidTime crate](https://img.shields.io/crates/v/rapid_time.svg)](https://crates.io/crates/rapid_time) [![RapidTime documentation](https://docs.rs/rapid_time/badge.svg)](https://docs.rs/rapid_time)
This crate defines two types: [`DateTime`](https://docs.rs/rapid_time/latest/rapid_time/struct.DateTime.html) and
[`Duration`](https://docs.rs/rapid_time/latest/rapid_time/struct.Duration.html), which are useful to model times in
combinatorial optimization problems.
* The smallest unit is a second.
* In addtion to actual times,
  [`DateTime::Earliest`](https://docs.rs/rapid_time/latest/rapid_time/struct.DateTime.html#associatedconstant.Earliest)
and [`DateTime::Latest`](https://docs.rs/rapid_time/latest/rapid_time/struct.DateTime.html#associatedconstant.Latest)
represents plus and minus infinity, respectively.
* Besides finite durations,
  [`Duration::Infinity`](https://docs.rs/rapid_time/latest/rapid_time/struct.Duration.html#associatedconstant.Infinity)
represents an infinite duration.
* [`Durations`](https://docs.rs/rapid_time/latest/rapid_time/struct.Duration.html)
can be added to or subtracted from [`DateTimes`](https://docs.rs/rapid_time/latest/rapid_time/struct.DateTime.html).
* [`Durations`](https://docs.rs/rapid_time/latest/rapid_time/struct.Duration.html)
can be added to or subtracted from each other and implement
[`Sum`](https://doc.rust-lang.org/std/iter/trait.Sum.html).
* [`DateTimes`](https://docs.rs/rapid_time/latest/rapid_time/struct.DateTime.html)
can be subtracted from each other to produce a
[`Duration`](https://docs.rs/rapid_time/latest/rapid_time/struct.Duration.html).
* [`Durations`](https://docs.rs/rapid_time/latest/rapid_time/struct.Duration.html)
and [`DateTimes`](https://docs.rs/rapid_time/latest/rapid_time/struct.DateTime.html)
are total ordered. (They implement
[`Ord`](https://doc.rust-lang.org/std/cmp/trait.Ord.html).)
* No negative durations are allowed.
* Both types are [`Copy`](https://doc.rust-lang.org/std/marker/trait.Copy.html)
and [`Clone`](https://doc.rust-lang.org/std/clone/trait.Clone.html).

# Usage

* Basic Usage
```rust
use rapid_time::{DateTime, Duration};
let tour_start = DateTime::new("2024-02-28T08:00:00");
let tour_length = Duration::new("100:00:00");
let tour_end = DateTime::new("2024-03-03T12:00:00");
assert_eq!(tour_start + tour_length, tour_end);
assert_eq!(tour_end - tour_start, tour_length);
assert_eq!(tour_end - tour_length, tour_start);
// Note that 2024 is a leap year.
```

* [`Earliest`](https://docs.rs/rapid_time/latest/rapid_time/struct.DateTime.html#associatedconstant.Earliest),
[`Latest`](https://docs.rs/rapid_time/latest/rapid_time/struct.DateTime.html#associatedconstant.Latest),
 and [`Infinity`](https://docs.rs/rapid_time/latest/rapid_time/struct.Duration.html#associatedconstant.Infinity)
```rust
# use rapid_time::{DateTime, Duration};
assert_eq!(DateTime::Earliest + Duration::new("10000:00:00"), DateTime::Earliest);
assert_eq!(DateTime::new("0000-01-01T00:00:00") + Duration::Infinity, DateTime::Latest);
assert_eq!(DateTime::Latest - DateTime::Earliest, Duration::Infinity);
assert_eq!(DateTime::Earliest + Duration::Infinity, DateTime::Latest);
```

* More [`Duration`](https://docs.rs/rapid_time/latest/rapid_time/struct.Duration.html)
```rust
# use rapid_time::{DateTime, Duration};
assert_eq!(Duration::new("1:00:00") + Duration::from_seconds(120), Duration::new("1:02:00"));
assert_eq!(Duration::new("100:00:00").in_sec().unwrap(), 100 * 3600);
assert_eq!(Duration::from_iso("P10DT2H00M59S").in_min().unwrap(), 10 * 24 * 60 + 2 * 60);
// Duration::from_seconds(10) - Duration::from_seconds(20); // panics
```

* More [`DateTime`](https://docs.rs/rapid_time/latest/rapid_time/struct.DateTime.html)
```rust
# use rapid_time::{DateTime, Duration};
assert_eq!(DateTime::new("2024-02-28T08:30").as_iso(), "2024-02-28T08:30:00");
// DateTime::new("2024-01-01T08:00") - DateTime::new("2024-01-01T09:00"); // panics
```


# Go Ecosystem

**Language:** Go · **Category:** Ecosystem

| Need   | Pick                       |
| ------ | -------------------------- |
| HTTP   | net/http + chi             |
| DB     | pgx, sqlc                  |
| Config | env + flag, viper if heavy |
| Log    | slog (stdlib)              |
| Test   | testify, testcontainers    |
| CLI    | cobra                      |

Default stdlib; add deps only when they earn it.

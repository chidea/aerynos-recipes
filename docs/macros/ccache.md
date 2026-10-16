# ccache macros

| Macro          | Description                                    |
| -------------- | ---------------------------------------------- |
| `%ccache_zero` | Zero out the ccache stats                      |
| `%ccache_stats`| Report ccache stats since last zero            |

Example: `%ccache_zero` → `%make` → `%ccache_stats`.

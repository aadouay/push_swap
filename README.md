# push_swap 🧠🧩

A fast and compact solution to the 42 push_swap challenge: sort a list of integers using two stacks (`A` and `B`) and a limited set of operations. The goal is to minimize the number of moves. Includes an optional `checker` that validates operation sequences. ✨

## 🚀 Overview
- `push_swap`: generates an efficient sequence of operations to sort inputs.
- `checker` (bonus): reads operations from `stdin` and reports `OK` or `KO`.
- Robust input parsing: supports quoted arguments, detects duplicates, empty strings, and out-of-range values. ❌

## 🧰 Prerequisites
- `gcc` and `make` on Linux

## 🛠️ Build
- Build main binary:
  ```zsh
  make
  ```
- Build bonus checker:
  ```zsh
  make bonus
  ```
- Clean objects / binaries:
  ```zsh
  make clean
  make fclean
  make re
  ```

Artifacts:
- `push_swap` (main)
- `checker` (bonus)
- Static libs: `libft/libft.a`, `ft_printf/printf.a`

## 🖥️ Usage
- Basic:
  ```zsh
  ./push_swap 2 1 3
  ```
  Prints a list of operations (one per line) that would sort the numbers.

- With quoted arguments:
  ```zsh
  ./push_swap "3 2 1 6 5"
  ```

- Validate with `checker`:
  ```zsh
  ARG="4 67 3 87 23"
  ./push_swap $ARG | ./checker $ARG
  # -> OK if sorted and stack B is empty; KO otherwise
  ```

- Error cases produce `Error` on `stderr` and exit:
  - Non-numeric inputs, overflows (outside 32-bit signed range), duplicates, or empty-only args. 🚫

## 🔤 Supported Operations
- Swap: `sa`, `sb`, `ss` 🔄
- Push: `pa`, `pb` ↔️
- Rotate up: `ra`, `rb`, `rr` 🔁
- Rotate down: `rra`, `rrb`, `rrr` 🔙

`checker` reads these ops from `stdin` via `get_next_line` and applies them.

## 🧪 Examples
- Small input:
  ```zsh
  ./push_swap 3 2 1
  # Example output (varies by algorithm):
  # pb
  # sa
  # pa
  ```

- Larger input:
  ```zsh
  ARG="90 52 19 2 3 7 5 4 8 1"
  ./push_swap $ARG | ./checker $ARG
  ```

## 📦 Project Structure
- Root
  - `push_swap.c`: main program; parses args, builds stack A, sorts, frees. 🧩
  - `ft_checker.c`: bonus checker; reads ops, applies to stacks, prints `OK/KO`. ✅
  - `check_arguments.c`, `creat_linked_list.c`: bonus helpers for checker.
  - `get_next_line.*`, `get_next_line.h`: input reading for checker.
  - `Makefile`: builds `push_swap` and `checker`, links local libs.
- `libft/` 🛠️
  - Core stack ops: `ft_push_*`, `ft_swap_*`, `ft_rotate_*`, `ft_reverse_rotate_*`.
  - Parsing & utils: `ft_split`, `ft_atoi` (overflow guard), `ft_valid_arguments`, `free_args`.
  - Sorting logic:
    - Small cases: `ft_sort_2nbr`, `ft_sort_list` (3 elems), `ft_full_sort` (dispatch).
    - General case: `ft_sort_stack` (chunk-based), `ft_last_sort` (max-to-top consolidation).
    - Helpers: `ft_insertion_sort`, `ft_find_position`, `ft_lstsize`, etc.
- `ft_printf/` 🖨️
  - Minimal `printf` implementation used for output.

## 🧠 Algorithm Notes
- Create a sorted reference array using insertion sort (`ft_insertion_sort`). 🧮
- Determine dynamic "chunk" ranges based on input size (`get_range`). 📏
- Push values from `A` to `B` within current range to cluster near-sorted groups (`ft_sort_stack`). 📦
- Keep `B` favoring descending order by swapping/rotating smartly (`ft_check_stack_b`). 🔧
- Final pass: bring maximums from `B` back to `A` in order (`ft_last_sort`). ⬆️
- Small N optimizations: tailored flows for 2–5 elements.

## 🧭 Input Rules
- Accepts space-separated integers; quoted strings are split internally. 🧵
- Rejects: non-integers, `int` overflow/underflow, duplicates, empty-only args.
- On invalid input: prints `Error` to `stderr` and exits. ❗

## 👤 Author
- `ayadouay` (42)

## 📄 License
- Not specified. If you need one, consider adding MIT. 📝

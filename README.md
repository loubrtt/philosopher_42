# Philosophers 🍝

A multithreading and synchronization project solving the classic dining philosophers problem using threads and mutexes.

## 📊 Score

**100/100** ✅

## 🎯 Description

Philosophers is a project that introduces concurrent programming concepts. It simulates the famous dining philosophers problem where philosophers alternate between eating, thinking, and sleeping. The challenge is to prevent deadlocks and ensure proper synchronization between threads.

## 🧠 The Dining Philosophers Problem

One or more philosophers sit at a round table with a large bowl of spaghetti in the center. There are as many forks as philosophers, placed between each pair of philosophers.

**Rules:**
- Philosophers alternate between **eating**, **thinking**, and **sleeping**
- A philosopher needs **two forks** (left and right) to eat
- After eating, they put down both forks and start sleeping
- After sleeping, they start thinking
- The simulation stops when a philosopher **dies of starvation**

**The challenge:** Philosophers don't speak to each other and don't know when another is about to die. How do you prevent them from starving?

## 🚀 Usage

```bash
./philo number_of_philosophers time_to_die time_to_eat time_to_sleep [number_of_times_each_philosopher_must_eat]
```

### Parameters

| Parameter | Description |
|-----------|-------------|
| `number_of_philosophers` | Number of philosophers (and forks) |
| `time_to_die` | Time (ms) a philosopher can survive without eating |
| `time_to_eat` | Time (ms) it takes to eat |
| `time_to_sleep` | Time (ms) spent sleeping |
| `[optional]` | Number of times each philosopher must eat before simulation stops |

### Examples

```bash
# 5 philosophers, die in 800ms, eat in 200ms, sleep in 200ms
./philo 5 800 200 200

# Same but stop after each philosopher eats 7 times
./philo 5 800 200 200 7

# Edge case: 1 philosopher (should die)
./philo 1 800 200 200

# No one should die
./philo 4 410 200 200
```

## 🛠️ Compilation

```bash
make        # Compile the program
make clean  # Remove object files
make fclean # Remove object files and executable
make re     # Recompile everything
```

## 📋 Program Output

Each state change is timestamped and logged:

```
0 1 has taken a fork
0 1 has taken a fork
0 1 is eating
200 1 is sleeping
200 2 has taken a fork
200 2 has taken a fork
200 2 is eating
400 1 is thinking
400 3 has taken a fork
400 3 has taken a fork
400 3 is eating
```

**Output format:**
```
timestamp_in_ms X has taken a fork
timestamp_in_ms X is eating
timestamp_in_ms X is sleeping
timestamp_in_ms X is thinking
timestamp_in_ms X died
```

Where `X` is the philosopher number (from 1 to number_of_philosophers).

## 🧵 Implementation Details

### Mandatory Part

**Tools used:**
- `pthread_create` - Create new threads
- `pthread_join` - Wait for thread termination
- `pthread_mutex_init` - Initialize mutex
- `pthread_mutex_destroy` - Destroy mutex
- `pthread_mutex_lock` - Lock mutex
- `pthread_mutex_unlock` - Unlock mutex

**Key concepts:**
- Each philosopher is a **thread**
- Each fork is protected by a **mutex**
- Data races are prevented with proper mutex usage
- Death checking runs in a separate monitoring thread/system
- Timestamps are calculated from program start


## 🔧 Key Challenges

### 1. Data Races
Prevent multiple threads from accessing shared data simultaneously using mutexes.

### 2. Deadlocks
Avoid situations where philosophers wait indefinitely for forks. Common solutions:
- Limit the number of philosophers that can pick up forks simultaneously
- Implement fork pickup order (odd/even philosophers)
- Resource hierarchy solution

### 3. Accurate Timing
Use `gettimeofday()` for precise millisecond timestamps and calculate elapsed time correctly.

### 4. Death Detection
Monitor each philosopher's last meal time to detect starvation before it's too late.

### 5. Clean Exit
Properly join all threads and destroy all mutexes on program termination.

## 🧪 Testing

### Basic Tests
```bash
# Should not die
./philo 5 800 200 200

# Should not die, stops after eating 7 times
./philo 5 800 200 200 7

# Should die (only 1 philosopher)
./philo 1 400 200 200

# Should die (not enough time to eat)
./philo 4 310 200 100
```

### Edge Cases
```bash
# Invalid arguments
./philo 0 800 200 200
./philo 5 0 200 200
./philo -5 800 200 200

# Large numbers
./philo 200 800 200 200

# Tight timing
./philo 4 410 200 200
```

### Testing for Issues
- **Data races:** Use `valgrind --tool=helgrind ./philo ...`
- **Memory leaks:** Use `valgrind --leak-check=full ./philo ...`
- **Death detection:** Verify death is printed within 10ms of actual death
- **Print order:** No messages after death message
- **Timestamp accuracy:** Check timestamps increase monotonically

## 🎯 Key Concepts Learned

- **Multithreading** with pthreads
- **Mutex synchronization** to prevent data races
- **Deadlock prevention** strategies
- **Race condition** detection and resolution
- **Time management** in concurrent programs
- **Thread lifecycle** management (creation, joining, cleanup)
- **Shared resource** protection

## 💭 What I Learned

- Deep understanding of thread synchronization
- Debugging concurrent programs
- The importance of mutex placement
- Timing precision in multithreaded environments
- Proper cleanup and resource management
- Testing strategies for non-deterministic programs
- Classic computer science problems and solutions

## 🚫 Common Pitfalls to Avoid

1. **Unprotected shared data** - Always use mutexes for shared variables
2. **Deadlock scenarios** - Ensure consistent lock ordering
3. **Printing without mutex** - Print operations must be atomic
4. **Inaccurate timing** - Don't use `sleep()`, implement precise `usleep()` wrapper
5. **Not checking death** - Monitor continuously, not just periodically
6. **Memory leaks** - Free all allocated memory and destroy all mutexes
7. **Delayed death detection** - Must detect and print death within 10ms

## 🏆 Project Success

Achieved 100/100 by:
- Zero data races (verified with helgrind)
- No memory leaks
- Accurate death detection (< 10ms)
- Proper mutex usage
- Clean thread management
- Handling all edge cases
- Clear, maintainable code structure

## 📚 Resources

- `man pthread_create`
- `man pthread_mutex_init`
- Wikipedia: Dining Philosophers Problem
- Operating Systems: Three Easy Pieces (OSTEP)

## 📝 License

This project is part of the 42 school curriculum.

---

Made with ☕ at 42

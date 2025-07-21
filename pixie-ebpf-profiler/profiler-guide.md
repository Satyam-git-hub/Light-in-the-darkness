# 🐝 eBPF Profiler Demo

A lightweight BCC-based eBPF profiler that traces stack samples from userspace applications like a Go program. Uses `perf_event` to sample call stacks and gives insights into application-level performance.

---

## 📦 Requirements

- Ubuntu 20.04 or 22.04
- Clang, BCC, Go installed

### 🔧 Install Dependencies

```bash
# Install BCC and development headers
sudo apt update
sudo apt install -y build-essential clang g++ g++-12 cmake \
  libbpfcc-dev libbcc-examples libbcc-dev bpfcc-tools \
  linux-headers-$(uname -r)


# Install Go (for the toy target app)
sudo apt install -y golang-go
````

---

## 🛠️ Build Instructions

```bash
git clone https://github.com/Satyam-git-hub/pixie-demos.git
cd pixie-demos/ebpf-profiler

# Build the profiler and toy Go app
make
```

This compiles:

* `perf_profiler`: The main C++ eBPF profiler using BCC
* `sqrt`: A small Go program to simulate workload

---

## 🚀 Running the Profiler

### 1. Launch the toy app in the background:

```bash
./sqrt &
```

### 2. Run the profiler on its PID:

```bash
sudo ./perf_profiler $(pgrep -f "sqrt") 10
```

> The `10` means "sample for 10 seconds".

---

## 📊 Example Output

```
Successfully deployed BPF profiler.
Collecting stack trace samples for 10 seconds.

3 main.main;runtime.main;runtime.goexit.abi0;
12 main.sqrtOf1e18;runtime.main;runtime.goexit.abi0;
13 main.sqrtOf1e39;runtime.main;runtime.goexit.abi0;
1 runtime.usleep.abi0;...;__x64_sys_nanosleep;entry_SYSCALL_64_after_hwframe;
```

---

## 📖 Output Explanation

Each line shows a **unique stack trace** and how often it appeared in the profiling session.

### Format:

```
<count> <leaf_function>;<parent_function>;...;
```

### Example:

```
13 main.sqrtOf1e39;runtime.main;runtime.goexit.abi0;
```

* This means the function `main.sqrtOf1e39()` was observed **13 times** during the sampling.
* The stack continues up through Go’s runtime (`runtime.main`, `goexit`).
* This is typically where the app was spending the most CPU time.

### System-level entries:

```
1 runtime.usleep.abi0;...;__x64_sys_nanosleep;entry_SYSCALL_64_after_hwframe;
```

* This indicates an idle goroutine or system sleep.
* Shows a complete syscall stack ending in Linux's scheduler.

### Warnings:

```
prog tag mismatch ... 
WARNING: cannot get prog tag, ignore saving source with program tag
```

* These are harmless BCC internal warnings — can be ignored unless persisting programs.

---

## 📁 File Structure

```
ebpf-profiler/
├── perf_profiler.cc      # The eBPF C++ profiler using BCC
├── toy_app/
│   └── sqrt.go           # Toy app that computes square roots
├── Makefile              # Build script
└── README.md             # This file ✨
```

---

## 💡 Customising

You can replace `toy_app/sqrt.go` with any Go (or C++) app you want to profile. Just make sure it’s CPU-intensive so you get meaningful stack traces.

---

## 🧠 Learn More

* BCC: [https://github.com/iovisor/bcc](https://github.com/iovisor/bcc)
* `perf_event_open`: Linux syscall for performance tracing
* `BPF_STACK_TRACE`: Special BCC helper for storing stack traces
* 🔬 Great for comparing kernel-level performance of microservices or AI workloads

---

## 🛡 License

MIT — feel free to hack and trace responsibly! 😄

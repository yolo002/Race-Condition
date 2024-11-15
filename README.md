# Race-Condition
solution


import threading

lock = threading.Lock()
shared_variable = 0

def increment():
    global shared_variable
    with lock:
        for _ in range(1000):
            shared_variable += 1

threads = [threading.Thread(target=increment) for _ in range(10)]
for t in threads:
    t.start()
for t in threads:
    t.join()

print(f"Final value of shared_variable: {shared_variable}")

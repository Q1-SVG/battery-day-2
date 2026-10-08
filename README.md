import numpy as np
import matplotlib.pyplot as plt
capacity_Ah = 2.0
R0 = 0.05       # 欧姆内阻
R1 = 0.03       # 极化内阻
C1 = 1000       # 极化电容
I = 2.0         # 1C 放电
dt = 1
t_end = 3600
def ocv(soc):
    return 3.0 + 1.2 * soc
soc = 1.0
V_RC = 0.0
time = [0]
voltage = [ocv(soc) - I * R0 - V_RC]
for t in range(1, t_end + 1, dt):
    soc = soc - I * dt / (capacity_Ah * 3600)
    if soc <= 0:
        break
    V_RC = V_RC * np.exp(-dt / (R1 * C1)) + I * R1 * (1 - np.exp(-dt / (R1 * C1)))
    voltage.append(ocv(soc) - I * R0 - V_RC)
    time.append(t)
plt.figure()
plt.plot(np.array(time) / 3600, voltage, label="Thevenin")
plt.xlabel("Time [h]")
plt.ylabel("Terminal voltage [V]")
plt.title("Thevenin Model")
plt.grid()
plt.legend()
plt.savefig("thevenin.png")
plt.show()

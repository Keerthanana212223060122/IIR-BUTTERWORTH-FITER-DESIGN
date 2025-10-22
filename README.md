# EXP 3 : IIR-BUTTERWORTH-FITER-DESIGN

# REG NO : 212223060122

## AIM: 
To design an IIR Butterworth filter  using SCILAB. 

## APPARATUS REQUIRED: 
PC installed with SCILAB. 

## PROGRAM (LPF): 
```python
clc;
clear;
close;

// User Inputs
wp = input('Enter the pass band frequency (Radians )= ');
ws = input('Enter the stop band frequency (Radians )= ');
alphap = input('Enter the pass band attenuation (dB)= ');
alphas = input('Enter the stop band attenuation (dB)= ');
T = input('Enter the Value of sampling Time= ');

// -------------------------
// Prewarping
// -------------------------
omegap = (2/T)*tan(wp/2);
mprintf("\nomegap = %f\n", omegap);

omegas = (2/T)*tan(ws/2);
mprintf("omegas = %f\n", omegas);

// -------------------------
// Order of Filter
// -------------------------
N = log10(((10^(0.1*alphas))-1) / ((10^(0.1*alphap))-1)) / (2*log10(omegas/omegap));
mprintf("N = %f\n", N);

N = ceil(N);
mprintf("Round off value of N = %d\n", N);

// -------------------------
// Cutoff Frequency
// -------------------------
omegac = omegap / (((10^(0.1*alphap)) - 1)^(1/(2*N)));
mprintf("omegac = %f\n", omegac);

// -------------------------
// Normalised Analog LPF Transfer Function
// -------------------------
mprintf("\nNormalised Analog LPF Transfer function H(S)=\n");
hs_Normalised = analpf(N, 'butt', [0,0], 1);
disp(hs_Normalised);

// -------------------------
// Actual Analog LPF
// -------------------------
mprintf("\nAnalog LPF Transfer function H(S)=\n");
hs_LPF = analpf(N, 'butt', [0,0], omegac);
disp(hs_LPF);

// -------------------------
// Digital LPF (Bilinear Transform)
// -------------------------
z = poly(0,'z');
Hz_LPF = horner(hs_LPF, (2/T)*((z-1)/(z+1))); 
mprintf("\nDigital LPF Transfer function H(Z)=\n");
disp(Hz_LPF);

// -------------------------
// Frequency Response (with grid)
// -------------------------
HW_LPF = frmag(Hz_LPF, 512);
w = 0:%pi/511:%pi;

plot(w/%pi, abs(HW_LPF),style=2);
xlabel(' Normalized Frequency (w/π)');
ylabel('Magnitude');
title('Frequency Response of Butterworth IIR LPF');
xgrid();
```


## PROGRAM (HPF): 
```python
clc;
clear;
close;

// User Inputs
wp = input('Enter the pass band frequency (Radians )= ');
ws = input('Enter the stop band frequency (Radians )= ');
alphap = input('Enter the pass band attenuation (dB)= ');
alphas = input('Enter the stop band attenuation (dB)= ');
T = input('Enter the Value of sampling Time= ');

// -------------------------
// Prewarping
// -------------------------
omegap = (2/T)*tan(wp/2);
mprintf("\nomegap = %f\n", omegap);

omegas = (2/T)*tan(ws/2);
mprintf("omegas = %f\n", omegas);

// -------------------------
// Order of Filter
// -------------------------
N = log10(((10^(0.1*alphas))-1) / ((10^(0.1*alphap))-1)) / (2*log10(omegap/omegas));
mprintf("N = %f\n", N);

N = ceil(N);
mprintf("Round off value of N = %d\n", N);

// -------------------------
// Cutoff Frequency
// -------------------------
omegac = omegap / (((10^(0.1*alphap)) - 1)^(1/(2*N)));
mprintf("omegac = %f\n", omegac);

// -------------------------
// Normalised Analog LPF Prototype
// -------------------------
mprintf("\nNormalised Analog LPF Transfer function H(S)=\n");
hs_Normalised = analpf(N, 'butt', [0,0], 1);
disp(hs_Normalised);

// -------------------------
// Analog HPF (LPF → HPF transformation)
// -------------------------
s = poly(0,'s');
hs_HPF = horner(hs_Normalised, omegac/s);
mprintf("\nAnalog HPF Transfer function H(S)=\n");
disp(hs_HPF);

// -------------------------
// Digital HPF (Bilinear Transform)
// -------------------------
z = poly(0,'z');
Hz_HPF = horner(hs_HPF, (2/T)*((z-1)/(z+1))); 
mprintf("\nDigital HPF Transfer function H(Z)=\n");
disp(Hz_HPF);

// -------------------------
// Frequency Response (with grid)
// -------------------------
HW_HPF = frmag(Hz_HPF, 512);
w = 0:%pi/511:%pi;

plot(w/%pi, abs(HW_HPF));
xlabel(' Normalized Frequency (w/π)');
ylabel('Magnitude');
title('Frequency Response of Butterworth IIR HPF');
xgrid();
```


## OUTPUT (LPF) : 

<img width="1919" height="953" alt="image" src="https://github.com/user-attachments/assets/da421e37-d368-4fbe-9f1e-8a0383c296df" />

<img width="1674" height="826" alt="image" src="https://github.com/user-attachments/assets/a04967cc-b476-481e-8388-078fbfdfbccf" />

## OUTPUT (HPF) : 

<img width="1920" height="976" alt="Screenshot 2025-10-22 120123" src="https://github.com/user-attachments/assets/0e4f082d-b23c-435a-af89-6342fd3f93e2" />

<img width="1650" height="818" alt="Screenshot 2025-10-22 120156" src="https://github.com/user-attachments/assets/6dc5e90f-0124-4051-a291-a9687977e9d2" />


## RESULT: 
Thus, design of Butterworth Low pass and High pass IIR filter waveforms were plotted and output was verified.

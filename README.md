# EXP 3 : IIR-BUTTERWORTH-FITER-DESIGN

# REG NO : 212223060122

## AIM: 
To design an IIR Butterworth filter  using SCILAB. 

## APPARATUS REQUIRED: 
PC installed with SCILAB. 

## PROGRAM (LPF): 
```python
clc ; 
close ; 
wp=input('Enter the pass band frequency (Radians )= ' ); 
ws=input('Enter the stop band frequency (Radians )= ' ); 
alphap=input( ' Enter the pass band attenuation (dB)=' ); 
alphas=input( ' Enter the stop band attenuation(dB)=' ); 
T=input('Enter the Value of sampling Time='); 
 
//Pre warping- Bilinear Transformation 
omegap=(2/T)*tan(wp/2); 
disp(omegap,'omegap='); 
omegas=(2/T)*tan(ws/2); 
disp(omegas,'omegas='); 
 
//Order of the filter 
 
N=log10(((10^(0.1*alphas))-1)/((10^(0.1*alphap))-1))/(2*log10(omegas/omegap)); 
disp(N,'N='); 
13 
 
N=ceil(N); 
 
disp(N,'Round off value of N='); 
 
 
//Cut off frequency 
omegac=omegap/(((10^(0.1*alphap)) -1)^(1/(2* N))); 
disp(omegac,'omegac='); 
 
disp('Normalised Analog LPF Transfer function H(S)='); 
hs_Normalised = analpf(N,'butt',[0,0],1); 
disp(hs_Normalised); 
 
disp('Analog LPF Transfer function H(S)='); 
hs= analpf(N,'butt',[0,0],omegac); 
disp(hs); 
 
 
z=poly(0,'z');//Defining variable z 
 
Hz=horner(hs,(2/ T)*((z -1)/(z+1)))// Bilinear Transformation 
disp('Digital LPF Transfer function H(Z)='); 
disp(Hz); 
 
 
HW=frmag(Hz,512); // Frequency response 
w=0:%pi/511:%pi ; 
plot(w/%pi,abs(HW)); 
 
xlabel(' Normalized Digital Frequency w'); 
ylabel('Magnitude '); 

 
title(' Frequency Response of Butterworth IIR LPF'); 
```


## PROGRAM (HPF): 
```python
clc;
clear;
close;

// User inputs
wp = input('Enter the pass band frequency (Radians )= ');
ws = input('Enter the stop band frequency (Radians )= ');
alphap = input('Enter the pass band attenuation (dB)= ');
alphas = input('Enter the stop band attenuation (dB)= ');
T = input('Enter the Value of sampling Time=');

// Pre-warping
omegap = (2/T)*tan(wp/2);
disp(omegap, 'omegap=');
omegas = (2/T)*tan(ws/2);
disp(omegas, 'omegas=');

// Order of the filter
N = log10(((10^(0.1*alphas))-1)/((10^(0.1*alphap))-1))/(2*log10(omegap/omegas));
disp(N, 'N=');
N = ceil(N);
disp(N, 'Round off value of N=');

// Cutoff frequency (for high-pass, based on stopband ratio)
omegac = omegas/(((10^(0.1*alphas)) -1)^(1/(2* N)));
disp(omegac, 'omegac=');

// Normalized Analog LPF
disp('Normalized Analog LPF Transfer function H(s)=');
hs_Normalised = analpf(N,'butt',[0,0],1);
disp(hs_Normalised);

// Actual Analog LPF
disp('Analog LPF Transfer function H(s)=');
hs = analpf(N,'butt',[0,0],omegac);
disp(hs);

// ---------- LPF to HPF Transformation ----------
s = poly(0,'s');
hs_hp = horner(hs, (omegac^2)/s);   // low-pass → high-pass transform

disp('Analog HPF Transfer function H(s)=');
disp(hs_hp);

// ---------- Bilinear Transformation ----------
z = poly(0,'z');
Hz = horner(hs_hp, (2/T)*((z - 1)/(z + 1)));

disp('Digital HPF Transfer function H(z)=');
disp(Hz);

// ---------- Frequency Response ----------
HW = frmag(Hz,512);
w = 0:%pi/511:%pi;

plot(w/%pi, abs(HW));
xlabel('Normalized Digital Frequency (×π rad/sample)');
ylabel('Magnitude');
title('Frequency Response of Butterworth IIR High-pass Filter');
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

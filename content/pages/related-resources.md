---
content_type: page
title: Related Resources
uid: 62c2fe9d-a7d9-e5de-8e33-cd8322c52851
---

This page contains engine performance specifications and scripts for determining engine exhaust composition.

[Engine Performance Specifications (XLSX)]({{< baseurl >}}/resources/engine_specs)

### Main program to call exhaust\_compo and plots results: exhaust\_compo\_main.m 

`%main driver for exhaust_compo   %Gasoline CHmOn;   m=1.87;n=0;   Lambda=[0.6:0.1:1.4];   [xco2,xh2o,xco,xh2,xo2,xn2]=exhaust_compo(m,n,Lambda);   h=plot(Lambda,[xco2,xh2o,xco,xh2,xo2],'-');   xlabel('\lambda');   ylabel('Exhaust Mole fractions');   legend('CO2','H2O','CO','H2','O2');`

### Function subroutine to calculate the composition: exhaust\_compo.m 

`function [xco2,xh2o,xco,xh2,xo2,xn2]=exhaust_compo(m,n,Lambda);   %calculate xhaust mole fractions   %For lean to stoichiometric combustion, assume complete burning   %For rich combustion, assume water gas equilibrium constant k=3.7   %Input: m,n are the hydrogen and oxygen to carbon ratios of the fuel   %Input: Lambda is the column vector of air equilvalence ratio   %Ouput are the mole fractions of the exhaust composition   k=3.7;   nL=length(Lambda);   xco2=zeros(nL,1);   xco=zeros(nL,1);   xh2o=zeros(nL,1);   xh2=zeros(nL,1);   xo2=zeros(nL,1);   xn2=zeros(nL,1);      for i=1:nL;   L=Lambda(i);   if (L<=0);error('Lambda must be positive');end;   if(L>=1);%=============== Lambda >=1 ==================   z=1+0.25*m-0.5*n;   Lm1=L-1;   co=0;h2=0;co2=1;h2o=0.5*m;   n2=z*3.773*L;   o2=Lm1*z;   else; %=============== Lambda < 1 ==================   z=n*(1-L)+2*L*(1+0.25*m);   B=2+(0.5*m-1)*k-(1-k)*z;   A=0.5*m*(1-k);   C=k*(1-z);   b=(-B+sqrt(B.*B-4*A*C))/(2*A);   a=b./(k+(1-k)*b);   co2=a;   co=1-a;   h2o=0.5*m*b;   h2=0.5*m*(1-b);   n2=(1+0.25*m-0.5*n)*3.773*L;   o2=0;   end %=============================================   total=co+h2+co2+h2o+n2+o2;   xco(i)=co./total;   xh2(i)=h2./total;   xco2(i)=co2./total;   xh2o(i)=h2o./total;   xo2(i)=o2./total;   xn2(i)=n2./total;   end`
# HABIT Software documentation

Documentation explaining several important points to take into account within the HABIT software. TIP: Check for TODOs in the scripts. Generally I have left that tag in order to know where things have to change.

# Solar Zenith Angle

In files `single_dec2env_FM.py` and `single_env2mod_FM.py`:

There is a variable called `landing_sol_of_year_corresonding_to_Ls` which corresponds to the value of the number of the landing sol corresponding to its Ls (check in http://www-mars.lmd.jussieu.fr/mars/time/solar_longitude.html). Also find it in the repo with the name  "ls_range and sol_range.png". You have to put the corresponding sol of the year and the software will  automatically interpolate the rest of the coming sols calculating the correct Ls from that day onwards. Clarification: You have to see the ls at that landing date, see the sol that it should correspond in the year and change the number to that sol. Put in this web page http://www-mars.lmd.jussieu.fr/mars/time/martian_time.html the landing date and it will give you the Ls at that moment and the sol of the year and directly put the sol of landing in the `landing_sol_of_year_corresonding_to_Ls` variable. 

e.g. If it lands on the 27/Apr/2021 you will get Ls: 37 which is sol 77 in the year. Hence `landing_sol_of_year_corresonding_to_Ls = 77` 


# Wind speed

The wind is calculated in two steps. First, the fluid temperature (Tf) and the heat flux (h) are calculated in lines 449 - 451 and saved to file in the `single_dec2env_FM.py` script. Next, as we are not sharing the m value in the output file, in the next script `single_env2mod_FM.py` you have to calculate the Tf and the h again for calculating the m because it is needed for the wind speed. In lines 346-348 you have the call to the wind methods with the different ATS values, fluid temperatures, m parameter, pressure and ATS number. The Pressure value that is given as an input, hence you must change it to the one obtained from the ancillary data in charge of it in line 23.  

Explaining the wind function: The first values are the Whitaker Optimal Ks and Epsilons. Then, depending on the ATS number it uses one or other, next, it makes sure that the m parameter is at least 2.2 and that the Tf is above 160. In case it doesn't fulfil this requirement, the wind and the heat transfer coefficient will be -1.0. Different calculations are done next calculating the surface temperature (Ts), the density (rho) using the rho_air auxiliary function, the Prandtl (Pr) using the auxiliary functions Cp_air, mu_air and k_air and the heat transfer coefficient is calculated. For the Pr calculation . After that, in the "while" function the Witacker reynolds is calculated and finally the wind speed (V_Wit) value is solved using the Reynolds value, the mu_air auxiliary function, the density and characteristic length (Lc). 

You can find the heat flux in the NV and MD file in columns 41, 42, and 43, the fluid temperature values in the NV and MD file in columns 44, 45 and 46, the wind speed values are being written only in the MD file in columns 51, 52 and 53, the heat transfer coefficient are being written in the MD file in columns 54, 55, and 56.

In the folder TES_PRESSURE/ I have created some scripts that used in my Master Thesis to give a solution in case there isn't any instrument measuring the pressure. In that script you can see how to take the pressure from the file `New1TES_ALL_DAY_FULLYEAR1_7188` (downloaded only the lat and Lon corresponding to Oxia Planum). See pictures attached for the full year calculated by TES for this lat/long.

# Ultraviolet (UV)

The `a_correction` value is the angle dependence correction factor, and we are using the one from Curiosity (a_correction = 5.675). This has to be changed in the script "single_dec2env_FM.py".

The `functionUV` function is a dummy function that gives 1.0. This has to be updated with the one obtained from the calibrations. See the formula and know how should it change. 

Most of the `R_UV_*` values are from Curiosity. The only real HABIT values are the ones corresponding to 25º (room temperature) so, its in range(23-25). So you will have to check with Thasshwin when he obtains the results from the calibrations.

# Ancillary data

Be aware in `single_dec2env_FM.py, single_dec2env_EQM.py, single_env2mod_FM.py and single_env2mod_EQM.py` scripts, that you have to change the values of platform_pitch, platform_yaw and platform_roll for the whatever value you receive from the SP. 

# Errors

The are two error values missing. The GTS error value and the UV error value.

# Obtain executable

To obtain the executable binary, you have to go to a Windows system, install the `pyinstaller` library and run `pyinstaller --onefile "C:\Users\Roberto Mantas\Documents\GitHub\habit-processing\main.py"` in the command line.
As it is over 25MB, it can't be uploaded to GitHub, if you want to have a look at the executable file, you can download it here: https://drive.google.com/file/d/1wD-X0TkZyh_Ee7mb3_7Z0YHVoUSIgLB8/view?usp=sharing

# Miscellanous

If Miracle does more tests with the EM, then in the `single_dec2env_EQM.py` script you have to comment the EQM values and uncomment the EM values (you will see what I mean in lines 261-361).

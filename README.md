# BetaPlaneRCE
Data, model input parameters, and code used to generate figures in Carstens and Wing (2022c, JAS). Data is from the System for Atmospheric Modeling (SAM, Khairoutdinov and Randall 2003).

Collection of beta-plane cloud-resolving model data used to study convective self-aggregation, including tropical cyclones and convectively-coupled equatorial waves, by Carstens and Wing (2022c). This repository contains model input parameters as plain text files for a series of simulations using the System for Atmospheric Modeling (SAM), developed by Marat Khairoutdinov at Stony Brook University. It also contains daily and daily-averaged data in standardized netCDF format from the SAM simulations used to generate figures, which will be described below. Additional data and code are available upon request.

# Model Input Parameters
There are three text files with parameters used to run the beta-plane simulations. These files are called grd (describing the vertical grid), snd (a reference sounding containing standard vertical profiles of altitude, pressure, potential temperature, water vapor mixing ratio, and wind), and prm (specific simulation settings including the sea surface temperature, center latitude, horizontal resolution, and frequency of model output).

The grd and snd files are identical across all five simulations in the study, reflecting the uniform initial conditions that each simulation begins from. The only parameter that varies between simulations in prm is latitude0, which sets the value of f corresponding to the center of the domain as an effective latitude. More details about each file are discussed below.

grd - This file simply lists the altitude of each vertical level in the model in meters. The first model level is at 37 m, with an initial vertical grid spacing of 75 m which gradually spaces out. Above 3 km, the vertical grid spacing is fixed at 500 m. Though the vertical domain extends to an altitude of approximately 33 km, the remaining vertical levels are inferred by the model from the uniform grid spacing above 3 km. There are 74 vertical levels in total.

snd - This text file contains 6 columns specifying the reference vertical profiles used to initialize each simulation. The first column is simply the altitude (same as grd). The second is the reference pressure in mb or hPa, where the model output is expressed as a perturbation from this reference pressure at each level. The third column is the potential temperature (K), and the fourth is the water vapor mixing ratio (g/kg). The fifth and sixth columns are the reference profiles of zonal and meridional wind, which are set to zero at every vertical level. That is, there is no mean flow or wind shear imposed in any simulation. This sounding is the equilibrium sounding from the 300 K small-domain non-rotating SAM simulation in the Radiative-Convective Equilibrium Model Intercomparison Project (RCEMIP, Wing et al. 2020).

prm - This file contains specific input parameters used to initialize simulations. The set of parameters beginning with "do" are boolean variables. These allow for the choice of settings such as interactive longwave and shortwave radiation, the use of a Coriolis parameter, interactive surface fluxes, and nudging of wind, water vapor, and temperature profiles. Below this, settings include the sea surface temperature, Coriolis parameter, solar insolation, horizontal grid spacing, and time step. The "nsave" parameters refer to the frequency of model output, expressed in the form of time steps. For example, nsave2D = 300 with a 12 second time step means that 2D (x,y) data will be averaged and written every hour.

# CRH.nc
This file contains snapshots of column relative humidity at daily intervals in each simulation (sim,x,y,t), with the order of sim being set by Table 1 in Carstens and Wing (2022c). The simulations begin with an identical random distribution of convection, and all other initial conditions are identical with the exception of the center effective latitude and magnitude of beta (meridional variability in the Coriolis parameter). Each simulation is run for 100 days.

# OLR.nc
This file contains snapshots of outgoing longwave radiation (OLR) at daily intervals in each simulation (sim,x,y,t), configured similarly to CRH.nc. The near-equatorial OLR data were used to examine convectively-coupled wave behavior. Code used to generate these analyses will be provided later in October.

# ZonalAverage_TimeSeries.nc
This file contains time series of zonally-averaged dynamic and thermodynamic variables used to characterize the mean state of the various beta-plane simulations, averaged daily. The variables are in dimensions of (sim,y,t), where sim is the simulation as in CRH.nc and OLR.nc, y is the meridional coordinate, and t is the day.  The data are as follows:

OLR - Outgoing longwave radiation (W/m^2)

PW - Column precipitable water (mm)

FMSE - Frozen moist static energy (J/m^2)

SF - 500 hPa subsidence fraction (unitless)

# TCTracks.nc
This file contains information on tropical cyclone tracks derived from an automated tracking algorithm built for these simulations, whose code will be added. TCs are identified using a local maximum threshold of 850 hPa relative vorticity and a persistence criteria. Some TCs in the file are initialized at hurricane intensity, owing to their re-emergence from either a Fujiwhara-type interaction or the meridional boundaries. For each simulation, TC tracks feature the following characteristics:

ttrack - Time step (6-hourly) of simulation when disturbance is identified

xtrack - Zonal coordinate (6-hourly) of simulation when disturbance is identified

ytrack - Meridional coordinate (6-hourly) of simulation when disturbance is identified

vmax - Maximum near-surface wind speed (m/s) at snapshot

lmi - Lifetime maximum intensity across entire TC track

# FMSEVarianceBudget.nc
This file contains daily and zonally-averaged time series (sim,y,t) of all feedbacks to the FMSE spatial variance budget of Wing and Emanuel (2014). These are used to generate Figures 3 and 5F in the manuscript. The feedbacks represent correlations between anomalies of FMSE and that particular variable in question.

VAR - Daily and domain-averaged variance of frozen moist static energy (FMSE, J^2/m^4)

VAR_TEN - Tendency of FMSE variance (i.e. what the budget solves for, J^2/(s m^4))

ADV - Advective feedback, representing changes in FMSE variance due to mesoscale circulations. This is calculated as a residual from VAR_TEN and DIAB (J^2/(s m^4))

DIAB - Diabatic feedback, the sum of the longwave, shortwave, and surface flux feedbacks. LW+SW+SEF (J^2/(s m^4))

SEF - Surface flux feedback, decomposed into latent and sensible heat flux components which can be broken down further (J^2/(s m^4))

LHF - Latent heat flux feedback, which adds into SEF (J^2/(s m^4))

SHF - Sensible heat flux feedback, which adds with LHF into SEF (J^2/(s m^4))

LW - Longwave radiative feedback, which can be decomposed into clear-sky and cloud components (J^2/(s m^4))

LW_CLOUD - Cloud longwave feedback, which adds into LW (J^2/(s m^4))

LW_CLEAR - Clear-sky longwave feedback, which adds with LW_CLOUD into LW (J^2/(s m^4))

SW - Shortwave radiative feedback, which can be decomposed into clear-sky and cloud components (J^2/(s m^4))

SW_CLOUD - Cloud shortwave feedback, which adds into SW (J^2/(s m^4))

SW_CLEAR - Clear-sky shortwave feedback, which adds with SW_CLOUD into SW (J^2/(s m^4))

LHF_EDDY - Eddy term of latent heat flux feedback, calculated using the product of disequilibrium and wind speed feedbacks (J^2/(s m^4))

LHF_WIND - Wind speed (WISHE) component of latent heat flux feedback (J^2/(s m^4))

LHF_DISEQ - Air-sea enthalpy disequilibrium component of latent heat flux feedback (J^2/(s m^4))

SHF_EDDY - Eddy term of sensible heat flux feedback, calculated using the product of disequilibrium and wind speed feedbacks (J^2/(s m^4))

SHF_WIND - Wind speed (WISHE) component of sensible heat flux feedback (J^2/(s m^4))

SHF_DISEQ - Air-sea enthalpy disequilibrium component of sensible heat flux feedback (J^2/(s m^4))

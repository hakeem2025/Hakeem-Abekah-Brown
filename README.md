# Hakeem-Abekah-Brown
Atmospheric Field Work
import xarray as xr
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
import cartopy.crs as ccrs
import cartopy.feature as cfeature
import matplotlib.ticker as mticker
from cartopy.mpl.gridliner import LONGITUDE_FORMATTER, LATITUDE_FORMATTER
from warnings import filterwarnings
filterwarnings('ignore')
 
xr.open_mfdataset('C:/Users/HP/Desktop/Hakeem/Hillaryparks8bKGmX0/*.nc')
sweden = xr.open_mfdataset('C:/Users/HP/Desktop/Hakeem/Hillaryparks8bKGmX0/*.nc')
sweden
sweden=sweden.where(sweden !=-99.0)
Sweden
sweden_data = sweden.sel(datetime=slice('2000','2010'))#,lon=(54),lat=(19))
sweden_precip = sweden_data['precip']
monthly_rainfall_totals = sweden_precip.resample(datetime= '1M').sum()
sweden_climatologies = sweden_precip.resample(datetime= '1M').mean()
 
 
annual_totals = sweden_precip.resample(datetime='1Y').sum()
annual_average = annual_totals.groupby('datetime.year')
annual_totals
sweden_co = xr.open_mfdataset('C:/Users/HP/Desktop/Hakeem/Hillaryparks8bKGmX0/*.nc')
sweden_precip1 = sweden_co['precip']
swed = sweden_precip1
annual_dry_days = (swed < 1).groupby('datetime.year').sum(dim= 'datetime')
annual_dry_days
 
# SPATIAL PLOTS FOR ANNUALL DRY DAYS
fig,ax=plt.subplots(5,2,figsize=(20,10), subplot_kw={'projection': ccrs.PlateCarree()})
ax=ax.flatten()
month_names=['2000','2001','2002','2003','2004','2005','2006','2007','2008','2009','2010']
for i in range(10):
   #ax[i].add_feature(cfeature.COASTLINE.with_scale('110m'),linewidth=0.5)
   ax[i].add_feature(cfeature.BORDERS,linewidth=2)
   ax[i].add_feature(cfeature.STATES,linewidth=0.5)
   #ax[i].add_feature(cfeature.OCEAN)
   ax[i].add_feature(cfeature.LAKES, color='blue')
   ax[i].add_feature(cfeature.RIVERS)
   ax[i].set_extent([-118.5,-86.7,32.0,14.75])
   ax[i].set_title(month_names[i])
   cb= ax[i].contourf(annual_dry_days.lon, annual_dry_days.lat, annual_dry_days[i], cmap='coolwarm', transform=ccrs.PlateCarree())
   color_bar=fig.add_axes([0.82,0.29,0.25,0.5])
fig.colorbar(cb,cax=color_bar,label='DRY DAYS(RR<1MM)')
fig.subplots_adjust(wspace=-0.55, top=0.93)
plt.title('ANNUAL DRY DAYS (RR<1mm)', fontweight='bold', fontsize='20')
# plt.savefig('done.png');

# Introduction #
PHP-SWEPH is a PHP extension to Astrodienst Swiss Ephemeris library. It's statically linked with libswe.a to implement one-to-one, C-to-PHP function mapping, no external binary executable required.

# Function Prototype/Implementation Status #
Currently, for those function exported by sweph.c, swedate.c, swehouse.c, swephlib.c are implemented. IMHO, it is sufficient for general astrological calculation. Eclipse functions from swecl.c are partially done, but not tested. The heliographic functions in swehel.c are not included.

For function prototypes and implementation status, please refer to the Google doc spreadsheet:
https://spreadsheets.google.com/ccc?key=0Aj0Hh9d0uhcCdFo4V1VpeHB6cGFuZVRtSFkxWHRnUUE&hl=zh_TW

# Installation #

**There's no binary distribution for download. Please get the source code and compile.**

Read [this](http://code.google.com/p/php-sweph/wiki/build) for build and install instructions.

# Usage #
**Example 1.**
```
<?php

    swe_set_ephe_path('/usr/local/share/sweph');

    list($y, $m, $d, $h, $mi, $s) = sscanf(gmdate("Y m d G i s"), "%d %d %d %d %d %d");
    $jul_ut = swe_julday($y, $m, $d, ($h + $mi / 60 + $s / 3600), SE_GREG_CAL);

    echo '<table>';
    echo "<tr><td colspan=5>Date: $y-$m-$d $h:$mi:$s</td></tr>";
    echo "<tr><td colspan=5>Julian Date: $jul_ut</td></tr>";

    for($i = SE_SUN; $i <= SE_VESTA; $i++)
    {
        if ($i == SE_EARTH) continue;

        echo '<tr>';
        echo '<td>' . swe_get_planet_name($i) . '</td>';

        $xx = swe_calc_ut($jul_ut, $i, SEFLG_SPEED);
        if ($xx['rc'] < 0) { // error calling swe_calc_ut();
            echo "<td colspan=4>" . $xx['serr'] . "</td>";
            continue;
        }
        echo '<td>' . $xx[0] . '</td>';
        echo '<td>' . $xx[1] . '</td>';
        echo '<td>' . $xx[2] . '</td>';
        echo '<td>' . $xx[3] . '</td>';
        echo '</tr>';
    }
    echo '</table>';
?>
```
[![giphy.gif](https://i.postimg.cc/MHH5Pqc5/giphy.gif)](https://postimg.cc/p5NKyNJ5)}
instrument { name = "Envelopes", overlay = true }

period = input (200, "Periodo", input.integer, 1)

deviation = input (0.05, "Desviación Estandar", input.double, 0.01, 300, 0.01)

source = input (5, "Fuente", input.string_selection,  inputs.titles_overlay)

fn     = input (1, "Media Movil", input.string_selection, averages.titles)

local method        = input(1, "Desarrollado por:", input.string_selection, { "Gabriel Arjona" })

input_group {
    "Linea Superior",
    upper_line_visible = input { default = true, type = input.plot_visibility },
    upper_line_color   = input { default = rgba(37,225,84,255), type = input.color },
    upper_line_width   = input { default = 1, type = input.line_width }
}

input_group {
    "Linea Central",
    middle_line_visible = input { default = true, type = input.plot_visibility },
    middle_line_color   = input { default = rgba(251,233,12,255), type = input.color },
    middle_line_width   = input { default = 1, type = input.line_width }
}

input_group {
    "Linea Inferior",
    lower_line_visible = input { default = true, type = input.plot_visibility },
    lower_line_color   = input { default = rgba(255,108,88,255), type = input.color },
    lower_line_width   = input { default = 1, type = input.line_width }
}

input_group {
    "Color de Relleno Alcista",
    fill_visible = input { default = true, type = input.plot_visibility },
    fill_color_bullish   = input { default = rgba(37,225,84,0.15), type = input.color }
}

input_group {
    "Color de Relleno Bajista",
    fill_visible = input { default = true, type = input.plot_visibility },
    fill_color_bearish   = input { default = rgba(255,108,88,0.15), type = input.color }
}


local sourceSeries = inputs [source]
local averageFunction = averages [fn]

middle =  averageFunction (sourceSeries, period)
offset = deviation / 1000 * middle

upper = middle + offset
lower = middle - offset

if fill_visible then
    fill { first = upper, second = middle, color = fill_color_bullish }
end

if fill_visible then
    fill { first = lower, second = middle, color = fill_color_bearish }
end

if upper_line_visible then
    plot (upper, "front.top line", upper_line_color, upper_line_width)
end

if lower_line_visible then
    plot (lower, "front.bottom line", lower_line_color, lower_line_width)
end

if middle_line_visible then
    plot (middle, "front.middle line", middle_line_color, middle_line_width)
end

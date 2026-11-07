# Roadmap until April 2027

We're trying to finish each step properly before starting the next one. For us, "finished" means we have a measured number to show for it, not just that it's put together.

| When | What we're doing | How we'll know it's done |
|---|---|---|
| December 2026 (winter break) | Mount the fan on the plywood baseplate with the foam gasket, take the blades off, then cut the 50 mm chuck disc, balance it, and centre it properly | We've logged speed tests with the bare hub and with the chuck, and the chuck spins without wobbling |
| January 2027 | Tune the controller again with the chuck on, fix the overshoot at start-up, and add a two-stage spin profile (slow spread, then faster thinning) that prints a summary after every run | The average speed stays within 1% of the target, and the variation stays under 2% |
| January to February | Test the coater with glycerin first, before spending money on resin | The coating looks even, and isn't thicker on one side |
| February | First resin coats on half-slides, at two different speeds with the same spin time. Measure thickness by weighing each slide before and after. | Two thickness numbers that roughly follow the expected relationship with speed |
| March | Make a new mask sized for half-slides, start exposing with the 405 nm torch, and read the exposure and resolution ladders on glass | We know the right exposure time for our resin and torch, and the smallest feature we can actually print |
| April | Do the same thing on a piece of silicon, and write up the full recipe | A developed pattern on silicon, and a written process we can show the IIT Bombay HackerFab group when we ask about getting real photoresist |

We're starting on glass because silicon reflects 405 nm light back into the resin. So an exposure time that works on glass will need adjusting for silicon, and glass is cheaper to make mistakes on.

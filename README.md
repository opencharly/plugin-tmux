# plugin-tmux

The `plugin-tmux` plugin candy of the [opencharly/charly](https://github.com/opencharly/charly)
candy library, as a standalone repo (the candy de-submodule cutover, plugin
kind). The Go module lives at `candy/plugin-tmux/` with module path
`github.com/opencharly/plugin-tmux/candy/plugin-tmux`; the charly resolver fetches this repo at the pinned tag and
the compiled-in wiring imports the module at that path.

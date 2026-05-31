## Site convention

- Blog posts: write in both English and Korean
- EN: `content/en/<section>/<post>.md`
- KO: `content/ko/<section>/<post>.md`
- If only one language available for a post, create symlink in the other language dir:
  `ln -s ../en/<section>/<post>.md content/ko/<section>/<post>.md`
- Both languages always need matching `_index.md` for each section
- Build: `cd doc && hugo`
- Preview: `cd doc && hugo server`

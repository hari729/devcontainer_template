# devcontainer_template
My template for containerised development with multi stage builds and development tools I use.

Each docker file is a layer with specific function.
base builds the most essential container that is required.
dev adds the development tools necessary, depend on the user on how to modify this. If you are using mason for lsp you probably don't need a all the things in this layer.
deps targets the dependencies required for this specific project.

This is just my first attempt at understanding containerized development workflow, I am sure there are better ways to do many things.



Refer https://github.com/MRo47/ros2_tools .

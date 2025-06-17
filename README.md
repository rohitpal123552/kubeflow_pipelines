# Step 1: Create and activate virtual environment and install dependencies
python3.10 -m venv kfp-env
source kfp-env/bin/activate

# Step 2: Upgrade pip + required build tools
pip install --upgrade pip setuptools wheel

# Step 3: Pre-install Cython < 3.0 (build requirement for PyYAML 5.4.1)
pip install "Cython<3.0.0"

# Step 4: Build and install PyYAML==5.4.1 manually
pip install --no-build-isolation PyYAML==5.4.1

# Step 5: Install all remaining dependencies from pyproject.toml
pip install \
  kfp==1.8.1 \
  urllib3<2.0 \
  requests-toolbelt<1.0.0 \
  google-auth==1.35.0 \
  protobuf==3.20.3


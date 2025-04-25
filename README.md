# Evo 2 Apptainer Container 

This repository hosts a prebuilt **Apptainer/Singularity container** for [Evo 2](https://github.com/ArcInstitute/evo2), a state-of-the-art DNA language model developed by the Arc Institute.

---

## 🔄 Files in this repository

- `sha256sum.txt` – SHA256 checksum for verifying the container  
- `evo2.sif` – Downloaded separately (see below)

---

## ✅ Getting Started

1. **Clone the official Evo 2 GitHub repository:**

   ```bash
   git clone --recurse-submodules https://github.com/ArcInstitute/evo2.git
   cd evo2
   ```

2. **Download the prebuilt `evo2.sif` container from Zenodo:**

   ```bash
   wget -O evo2.sif "https://zenodo.org/records/15194473/files/evo2.sif?download=1"
   ```

   Or download it manually here:  
   👉 [Download evo2.sif (12.3GB)](https://zenodo.org/records/15194473/files/evo2.sif?download=1)

3. **Create a directory for model weights:**

   ```bash
   mkdir -p models
   ```

4. **Run Evo 2 using the Apptainer container:**

   ```bash
   singularity exec \
       --nv \
       --bind $PWD:/app \
       --bind ./models:/root/.cache/huggingface \
       ./evo2.sif \
       python3 ./test/test_evo2.py \
       --model_name evo2_7b
   ```

   > 🧠 This will use GPU acceleration (`--nv`), bind your local code and model directories, and run inference using the `evo2_7b` model.

---

## 🔄 Building the Container Yourself (Optional)

If you prefer building from source using Docker:

```bash
git clone --branch add-dockerfile https://github.com/victornemeth/evo2.git
cd evo2
docker build -t evo2 .
singularity build evo2.sif docker-daemon://evo2:latest
```

---

## 🔒 Verifying the Container

To verify the container file:

```bash
diff <(sha256sum evo2.sif) sha256sum.txt && echo "✅ Match!" || echo "❌ Mismatch!"
```

---

Note:

You can skip the warning about flash-attn, it uses flash attn build in vortex, but apptainer did not work if not also installed by pip.

---

## 📄 License

This repo contains only the container checksum. For licensing and usage terms, refer to the [official Evo 2 repository](https://github.com/ArcInstitute/evo2).

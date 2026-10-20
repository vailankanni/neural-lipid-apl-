
# neural-lipid-apl 🧬

Can a neural network predict how much space a lipid takes up in a membrane? That's what this project is about.

## What's this?

Area per lipid (APL) is basically how much room each lipid gets in a bilayer. It's one of the first things people check when simulating membranes, and it changes depending on what lipids are in the membrane, the temperature, and which force field you use.

I work with lipid membranes in my PhD (mostly coarse-grained MARTINI 3 simulations), and I wanted to try something new: train a neural network in PyTorch to predict APL straight from simulation conditions, and see how it stacks up against a Gaussian process, which is what I usually use.

## The data

Everything comes from the [NMRlipids Databank](https://github.com/NMRLipids/Databank), a really cool open collection of atomistic membrane simulations from groups all over the world. I only use the precomputed APL values and simulation info, so there's no need to download any trajectories.

For each simulation, the model gets:
- the lipid mix (mole fractions)
- the temperature
- the force field (CHARMM36, Slipids, etc.)

and tries to predict the area per lipid (Å²).

## What I did

- Built a small neural network (MLP) in PyTorch and wrote my own training loop with early stopping, so it doesn't just memorize the data
- Trained 5 networks with different random seeds and used their spread as a rough uncertainty estimate
- Ran a Gaussian process (scikit-learn) on the exact same data splits
- Compared them with 5-fold cross-validation (R² and RMSE)

## Results

| Model | R² | RMSE (Å²) |
|---|---|---|
| Neural network (ensemble) | TBD | TBD |
| Gaussian process | TBD | TBD |

Plots are in `figures/`. Spoiler: I'm curious whether the neural net can keep up with a GP on a dataset this small!

## Try it yourself

```bash
git clone https://github.com/vailankanni/neural-lipid-apl.git
cd neural-lipid-apl
pip install -r requirements.txt

python src/build_dataset.py   # grab the data
python src/benchmark.py       # train and compare
```

No GPU needed, it runs fine on a regular CPU.

## What's in here

```
src/build_dataset.py   # pulls APL + info from the databank
src/model.py           # the neural network
src/train.py           # training loop + ensemble
src/benchmark.py       # neural net vs GP showdown
figures/               # plots
```

## Credit

Huge thanks to the NMRlipids community for making their data open. If you use it, please cite their paper:

> Kiirikki, A. M., et al. NMRlipids Databank makes data-driven analysis of biomembrane properties accessible for all. *Nature Communications* (2024).

## About me

I'm Val, a PhD candidate in Chemical and Biochemical Engineering at Rutgers, working on simulations of lipid membranes and proteins. Say hi on [GitHub](https://github.com/vailankanni) or check out my [Google Scholar](https://scholar.google.com/citations?user=Co7ZhWoAAAAJ).

MIT License

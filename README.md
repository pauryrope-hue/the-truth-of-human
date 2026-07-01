"""
================================================================================
UIM Unified Intelligence Core v7.0
Ultimate Diagnostics Artifact
--------------------------------------------------------------------------------
[Canonical Conceptual Simulation Record]

This program is a conceptual research artifact based on the UIM hypothesis:

    The universe may be interpreted as a self-verifying informational system.
    In this model, consistency is represented as an energy minimum.

Important boundary:

    This kernel is not a mathematical proof engine.
    It does not prove, solve, or resolve any Millennium Prize Problem.
    It provides a reproducible simulation environment for UIM-style
    dissipation, synchronization, convergence, entropy diagnostics,
    and conceptual mapping.

Conceptual mapping suite:

    P vs NP        -> Mode A: metaphorical dissipation of inconsistent paths
    Riemann        -> Mode B: visual attraction toward Re = 0.5
    Navier-Stokes  -> Mode B: relaxation of informational congestion
    Yang-Mills     -> Mode B: structural stabilization analogy
    BSD            -> Mode B: simplified coherence alignment
    Hodge          -> Mode B: topological consistency alignment analogy

This artifact records convergence behavior inside the designed UIM dynamical
system. It does not establish equivalence between that dynamical system and the
original mathematical problems.
================================================================================
"""

import math
import textwrap
from typing import Any, Callable, Dict, Iterable, Literal, Optional

import matplotlib.pyplot as plt
import numpy as np


Mode = Literal["A", "B"]
Component = Literal["real", "imag", "abs"]
InitialPolicy = Literal["same", "different", "chain"]


class UIM_Ultimate_Diagnostics_Artifact:
    """
    UIM Unified Intelligence Core v7.0
    Ultimate Diagnostics Artifact

    This object integrates:
        - reproducible initialization
        - Mode A / Mode B physical kernel
        - static target convergence
        - dynamic target tracking
        - target / physical / consistency / coherence energy
        - entropy and phase-space entropy
        - deterministic replay diagnostics
        - fixed-point self-check
        - transient FFT analysis
        - parameter sensitivity sweep
        - conceptual mapping suite
        - canonical simulation reporting
        - visualization

    Boundary:
        This is not a proof engine.
        It does not prove or resolve any mathematical theorem.
        Suite labels are conceptual mappings only.
    """

    VERSION = "7.0"
    VALID_MODES = {"A", "B"}

    # ------------------------------------------------------------------
    # Construction
    # ------------------------------------------------------------------

    def __init__(
        self,
        num_nodes: int = 100,
        dt: float = 0.01,
        g: float = 2.5,
        coupling: float = 0.1,
        real_restore_rate: float = 2.5,
        imag_decay_rate: float = 1.0,
        mode_a_decay_rate: float = 0.1 * np.exp(5.0),
        mode_a_linear_decay_rate: float = 0.0,
        seed: int = 42,
        initial_real_span: float = 1.5,
        initial_imag_span: float = 3.0,
    ):
        self._validate_parameters(
            num_nodes=num_nodes,
            dt=dt,
            g=g,
            coupling=coupling,
            real_restore_rate=real_restore_rate,
            imag_decay_rate=imag_decay_rate,
            mode_a_decay_rate=mode_a_decay_rate,
            mode_a_linear_decay_rate=mode_a_linear_decay_rate,
            seed=seed,
            initial_real_span=initial_real_span,
            initial_imag_span=initial_imag_span,
        )

        self.params: Dict[str, Any] = {
            "num_nodes": int(num_nodes),
            "dt": float(dt),
            "g": float(g),
            "coupling": float(coupling),
            "real_restore": float(real_restore_rate),
            "imag_decay": float(imag_decay_rate),
            "mode_a_decay": float(mode_a_decay_rate),
            "mode_a_linear_decay": float(mode_a_linear_decay_rate),
            "initial_real_span": float(initial_real_span),
            "initial_imag_span": float(initial_imag_span),
        }

        self.seed = int(seed)

        self.origin = 0.0 + 0.0j
        self.consistency_target = 0.5 + 0.0j

        self.initial_nodes = self._generate_nodes(self.seed)
        self.nodes = self.initial_nodes.copy()

        self.history: Dict[str, np.ndarray] = {}
        self.target_logs: Dict[str, np.ndarray] = {}
        self.energy_logs: Dict[str, Dict[str, np.ndarray]] = {}
        self.frequency_logs: Dict[str, Dict[str, Any]] = {}
        self.run_metadata: Dict[str, Dict[str, Any]] = {}

        self.conceptual_mappings = self._build_conceptual_mappings()

    # ------------------------------------------------------------------
    # Validation
    # ------------------------------------------------------------------

    @staticmethod
    def _validate_positive_int(name: str, value: int) -> None:
        if not isinstance(value, int) or isinstance(value, bool):
            raise TypeError(f"{name} must be an integer.")
        if value <= 0:
            raise ValueError(f"{name} must be positive.")

    @staticmethod
    def _validate_integer(name: str, value: int) -> None:
        if not isinstance(value, int) or isinstance(value, bool):
            raise TypeError(f"{name} must be an integer.")

    @staticmethod
    def _validate_real(name: str, value: float) -> None:
        if isinstance(value, bool):
            raise TypeError(f"{name} must be a real number.")
        if not isinstance(value, (int, float, np.integer, np.floating)):
            raise TypeError(f"{name} must be a real number.")
        if not np.isfinite(value):
            raise ValueError(f"{name} must be finite.")

    @classmethod
    def _validate_positive_float(cls, name: str, value: float) -> None:
        cls._validate_real(name, value)
        if value <= 0:
            raise ValueError(f"{name} must be positive.")

    @classmethod
    def _validate_non_negative_float(cls, name: str, value: float) -> None:
        cls._validate_real(name, value)
        if value < 0:
            raise ValueError(f"{name} must be non-negative.")

    @classmethod
    def _validate_parameters(
        cls,
        num_nodes: int,
        dt: float,
        g: float,
        coupling: float,
        real_restore_rate: float,
        imag_decay_rate: float,
        mode_a_decay_rate: float,
        mode_a_linear_decay_rate: float,
        seed: int,
        initial_real_span: float,
        initial_imag_span: float,
    ) -> None:
        cls._validate_positive_int("num_nodes", num_nodes)
        cls._validate_positive_float("dt", dt)
        cls._validate_non_negative_float("g", g)
        cls._validate_non_negative_float("coupling", coupling)
        cls._validate_non_negative_float("real_restore_rate", real_restore_rate)
        cls._validate_non_negative_float("imag_decay_rate", imag_decay_rate)
        cls._validate_non_negative_float("mode_a_decay_rate", mode_a_decay_rate)
        cls._validate_non_negative_float(
            "mode_a_linear_decay_rate",
            mode_a_linear_decay_rate,
        )
        cls._validate_integer("seed", seed)
        cls._validate_positive_float("initial_real_span", initial_real_span)
        cls._validate_positive_float("initial_imag_span", initial_imag_span)

    @classmethod
    def _validate_mode(cls, mode: str) -> None:
        if mode not in cls.VALID_MODES:
            raise ValueError(f"Unknown mode: {mode}. Use 'A' or 'B'.")

    @staticmethod
    def _validate_key(key: str) -> None:
        if not isinstance(key, str) or not key.strip():
            raise ValueError("key must be a non-empty string.")

    @staticmethod
    def _validate_ratio_interval(start_ratio: float, end_ratio: float) -> None:
        if isinstance(start_ratio, bool) or isinstance(end_ratio, bool):
            raise TypeError("ratios must be real numbers.")

        if not isinstance(start_ratio, (int, float, np.integer, np.floating)):
            raise TypeError("start_ratio must be a real number.")

        if not isinstance(end_ratio, (int, float, np.integer, np.floating)):
            raise TypeError("end_ratio must be a real number.")

        if not np.isfinite(start_ratio) or not np.isfinite(end_ratio):
            raise ValueError("ratios must be finite.")

        if not (0.0 <= float(start_ratio) < float(end_ratio) <= 1.0):
            raise ValueError("Require 0.0 <= start_ratio < end_ratio <= 1.0.")

    @staticmethod
    def _validate_bins(bins: int) -> None:
        if not isinstance(bins, int) or isinstance(bins, bool):
            raise TypeError("bins must be an integer.")
        if bins <= 1:
            raise ValueError("bins must be greater than 1.")

    # ------------------------------------------------------------------
    # Manifest
    # ------------------------------------------------------------------

    def manifest(self) -> str:
        mapping_text = "\n".join(
            f"        - {name}: Mode {meta['mode']} | {meta['description']}"
            for name, meta in self.conceptual_mappings.items()
        )

        return textwrap.dedent(f"""
        ================================================================================
        UIM Unified Intelligence Core v{self.VERSION}
        --------------------------------------------------------------------------------
        [System]
        Ultimate Diagnostics Artifact

        [Core Axiom - Interpretive]
        The universe is interpreted as a self-verifying informational system.
        In this model, consistency is represented as an energy minimum.

        [Boundary]
        This artifact is not a mathematical proof engine.
        It does not solve, prove, or resolve any Millennium Prize Problem.
        It records behavior of a designed UIM-style dynamical system.

        [Mode A]
        Dissipative convergence toward a target.
        Default target = 0+0j.

        [Mode B]
        Consistency-centered convergence toward a target.
        Default target = 0.5+0j.

        [Conceptual Mapping Suite]
{mapping_text}

        [Canonical Record Policy]
        The canonical report is a reproducible simulation record only.
        It must not be interpreted as a formal proof of the named problems.

        [Diagnostics]
        - fixed-point self-check
        - deterministic replay check
        - target-energy diagnostics
        - entropy diagnostics
        - phase-space entropy
        - transient rFFT
        - parameter sensitivity sweep
        - dynamic target tracking

        [Parameters]
        Nodes                    : {self.params["num_nodes"]}
        dt                       : {self.params["dt"]}
        G-Constant               : {self.params["g"]}
        Coupling                 : {self.params["coupling"]}
        Real Restore Rate        : {self.params["real_restore"]}
        Imaginary Decay Rate     : {self.params["imag_decay"]}
        Mode A Decay Rate        : {self.params["mode_a_decay"]}
        Mode A Linear Decay Rate : {self.params["mode_a_linear_decay"]}
        Seed                     : {self.seed}
        ================================================================================
        """).strip()

    def boot(self) -> "UIM_Ultimate_Diagnostics_Artifact":
        print(self.manifest())
        return self

    # ------------------------------------------------------------------
    # Conceptual mapping suite
    # ------------------------------------------------------------------

    @staticmethod
    def _build_conceptual_mappings() -> Dict[str, Dict[str, str]]:
        return {
            "P_vs_NP": {
                "mode": "A",
                "description": "metaphorical dissipation of inconsistent paths",
                "note": "This is not a proof of P vs NP.",
            },
            "Riemann": {
                "mode": "B",
                "description": "visual attraction toward Re = 0.5",
                "note": "This is not a proof of the Riemann Hypothesis.",
            },
            "Navier_Stokes": {
                "mode": "B",
                "description": "relaxation of informational congestion",
                "note": "This is not a proof of Navier-Stokes regularity.",
            },
            "Yang_Mills": {
                "mode": "B",
                "description": "structural stabilization analogy",
                "note": "This is not a proof of Yang-Mills mass gap.",
            },
            "BSD": {
                "mode": "B",
                "description": "simplified coherence alignment",
                "note": "This is not a proof of the Birch and Swinnerton-Dyer conjecture.",
            },
            "Hodge": {
                "mode": "B",
                "description": "topological consistency alignment analogy",
                "note": "This is not a proof of the Hodge conjecture.",
            },
        }

    # ------------------------------------------------------------------
    # Initialization / spawning
    # ------------------------------------------------------------------

    def _generate_nodes(self, seed: int) -> np.ndarray:
        rng = np.random.default_rng(seed)
        n = self.params["num_nodes"]

        real = self.consistency_target.real + (
            rng.random(n) - 0.5
        ) * self.params["initial_real_span"]

        imag = self.consistency_target.imag + (
            rng.random(n) - 0.5
        ) * self.params["initial_imag_span"]

        return real + 1j * imag

    def reset(self) -> "UIM_Ultimate_Diagnostics_Artifact":
        self.nodes = self.initial_nodes.copy()
        return self

    def reseed(self, seed: int) -> "UIM_Ultimate_Diagnostics_Artifact":
        self._validate_integer("seed", seed)
        self.seed = int(seed)
        self.initial_nodes = self._generate_nodes(self.seed)
        self.reset()
        return self

    def clear_logs(self) -> "UIM_Ultimate_Diagnostics_Artifact":
        self.history.clear()
        self.target_logs.clear()
        self.energy_logs.clear()
        self.frequency_logs.clear()
        self.run_metadata.clear()
        return self

    def _spawn(
        self,
        overrides: Optional[Dict[str, Any]] = None,
    ) -> "UIM_Ultimate_Diagnostics_Artifact":
        config = {
            "num_nodes": self.params["num_nodes"],
            "dt": self.params["dt"],
            "g": self.params["g"],
            "coupling": self.params["coupling"],
            "real_restore_rate": self.params["real_restore"],
            "imag_decay_rate": self.params["imag_decay"],
            "mode_a_decay_rate": self.params["mode_a_decay"],
            "mode_a_linear_decay_rate": self.params["mode_a_linear_decay"],
            "seed": self.seed,
            "initial_real_span": self.params["initial_real_span"],
            "initial_imag_span": self.params["initial_imag_span"],
        }

        if overrides:
            config.update(overrides)

        return self.__class__(**config)

    # ------------------------------------------------------------------
    # Targets
    # ------------------------------------------------------------------

    def default_target(self, mode: Mode) -> complex:
        self._validate_mode(mode)
        return self.origin if mode == "A" else self.consistency_target

    def _mode_for_key(self, key: str) -> Mode:
        if key in self.run_metadata:
            mode = self.run_metadata[key]["mode"]
            self._validate_mode(mode)
            return mode

        if key in {"A", "B"}:
            return key

        raise RuntimeError(f"No mode metadata found for key: {key}")

    # ------------------------------------------------------------------
    # Energy / entropy
    # ------------------------------------------------------------------

    @staticmethod
    def physical_energy(nodes: np.ndarray) -> float:
        return float(np.sum(np.abs(nodes) ** 2))

    def consistency_energy(self, nodes: np.ndarray) -> float:
        return float(np.sum(np.abs(nodes - self.consistency_target) ** 2))

    @staticmethod
    def coherence_energy(nodes: np.ndarray) -> float:
        mean_state = np.mean(nodes)
        return float(np.sum(np.abs(nodes - mean_state) ** 2))

    @staticmethod
    def target_energy(nodes: np.ndarray, target: complex) -> float:
        return float(np.sum(np.abs(nodes - target) ** 2))

    @classmethod
    def entropy_1d(cls, values: np.ndarray, bins: int = 20) -> float:
        cls._validate_bins(bins)

        counts, _ = np.histogram(values, bins=bins, density=False)
        total = np.sum(counts)

        if total == 0:
            return 0.0

        probs = counts.astype(float) / float(total)
        probs = probs[probs > 0]

        return float(-np.sum(probs * np.log(probs)))

    @classmethod
    def phase_space_entropy(cls, nodes: np.ndarray, bins: int = 20) -> float:
        cls._validate_bins(bins)

        counts, _, _ = np.histogram2d(
            nodes.real,
            nodes.imag,
            bins=bins,
            density=False,
        )

        total = np.sum(counts)

        if total == 0:
            return 0.0

        probs = counts.ravel().astype(float) / float(total)
        probs = probs[probs > 0]

        return float(-np.sum(probs * np.log(probs)))

    def _energy_snapshot_from(
        self,
        nodes: np.ndarray,
        target: complex,
    ) -> Dict[str, float]:
        n = self.params["num_nodes"]

        phys = self.physical_energy(nodes)
        cons = self.consistency_energy(nodes)
        coh = self.coherence_energy(nodes)
        targ = self.target_energy(nodes, target)

        return {
            "phys": phys,
            "cons": cons,
            "coh": coh,
            "target": targ,
            "phys_mean": phys / n,
            "cons_mean": cons / n,
            "coh_mean": coh / n,
            "target_mean": targ / n,
        }

    @staticmethod
    def _new_energy_log(steps: int) -> Dict[str, np.ndarray]:
        return {
            "phys": np.zeros(steps + 1),
            "cons": np.zeros(steps + 1),
            "coh": np.zeros(steps + 1),
            "target": np.zeros(steps + 1),
            "phys_mean": np.zeros(steps + 1),
            "cons_mean": np.zeros(steps + 1),
            "coh_mean": np.zeros(steps + 1),
            "target_mean": np.zeros(steps + 1),
        }

    def _record_energy(
        self,
        energy: Dict[str, np.ndarray],
        index: int,
        nodes: np.ndarray,
        target: complex,
    ) -> None:
        snapshot = self._energy_snapshot_from(nodes, target)

        for key, value in snapshot.items():
            energy[key][index] = value

    # ------------------------------------------------------------------
    # Physics kernel
    # ------------------------------------------------------------------

    def _apply_rotation_about(
        self,
        nodes: np.ndarray,
        center: complex,
    ) -> np.ndarray:
        deviation = nodes - center
        intensity = np.abs(deviation) ** 2
        phase = self.params["g"] * intensity * self.params["dt"]

        return center + deviation * np.exp(1j * phase)

    def _step_mode_a(
        self,
        nodes: np.ndarray,
        target: complex,
    ) -> np.ndarray:
        rotated = self._apply_rotation_about(nodes, target)
        intensity = np.abs(nodes - target) ** 2

        nonlinear_collapse = np.exp(
            -self.params["mode_a_decay"] * intensity * self.params["dt"]
        )

        linear_collapse = np.exp(
            -self.params["mode_a_linear_decay"] * self.params["dt"]
        )

        return target + (rotated - target) * nonlinear_collapse * linear_collapse

    def _step_mode_b(
        self,
        nodes: np.ndarray,
        target: complex,
    ) -> np.ndarray:
        rotated = self._apply_rotation_about(nodes, target)

        field_mean = np.mean(rotated)
        coupled = rotated + self.params["coupling"] * (
            field_mean - rotated
        ) * self.params["dt"]

        re_decay = np.exp(-self.params["real_restore"] * self.params["dt"])
        im_decay = np.exp(-self.params["imag_decay"] * self.params["dt"])

        new_re = target.real + (coupled.real - target.real) * re_decay
        new_im = target.imag + (coupled.imag - target.imag) * im_decay

        return new_re + 1j * new_im

    def _next_nodes(
        self,
        nodes: np.ndarray,
        mode: Mode,
        target: complex,
    ) -> np.ndarray:
        self._validate_mode(mode)

        if mode == "A":
            return self._step_mode_a(nodes, target)

        return self._step_mode_b(nodes, target)

    def step(
        self,
        mode: Mode = "B",
        target: Optional[complex] = None,
    ) -> "UIM_Ultimate_Diagnostics_Artifact":
        active_target = self.default_target(mode) if target is None else complex(target)
        self.nodes = self._next_nodes(self.nodes, mode, active_target)
        return self

    # ------------------------------------------------------------------
    # Run
    # ------------------------------------------------------------------

    def run(
        self,
        steps: int = 1000,
        mode: Mode = "B",
        reset: bool = True,
        key: Optional[str] = None,
        target_func: Optional[Callable[[int], complex]] = None,
        metadata: Optional[Dict[str, Any]] = None,
        callback: Optional[
            Callable[[int, np.ndarray, Dict[str, float]], None]
        ] = None,
    ) -> "UIM_Ultimate_Diagnostics_Artifact":
        if not isinstance(steps, int) or isinstance(steps, bool):
            raise TypeError("steps must be an integer.")

        if steps < 1:
            raise ValueError("steps must be at least 1.")

        self._validate_mode(mode)

        run_key = key if key is not None else mode
        self._validate_key(run_key)

        if reset:
            self.reset()

        n = self.params["num_nodes"]
        history = np.zeros((steps + 1, n), dtype=complex)
        target_log = np.zeros(steps + 1, dtype=complex)
        energy = self._new_energy_log(steps)

        def active_target(step_index: int) -> complex:
            if target_func is None:
                return self.default_target(mode)
            return complex(target_func(step_index))

        target_0 = active_target(0)

        history[0] = self.nodes.copy()
        target_log[0] = target_0
        self._record_energy(energy, 0, self.nodes, target_0)

        if callback is not None:
            callback(
                0,
                self.nodes.copy(),
                self._energy_snapshot_from(self.nodes, target_0),
            )

        for step_index in range(1, steps + 1):
            target = active_target(step_index)
            self.nodes = self._next_nodes(self.nodes, mode, target)

            history[step_index] = self.nodes.copy()
            target_log[step_index] = target
            self._record_energy(energy, step_index, self.nodes, target)

            if callback is not None:
                callback(
                    step_index,
                    self.nodes.copy(),
                    self._energy_snapshot_from(self.nodes, target),
                )

        self.history[run_key] = history
        self.target_logs[run_key] = target_log
        self.energy_logs[run_key] = energy

        base_metadata = {
            "key": run_key,
            "mode": mode,
            "kind": "direct_run",
            "steps": steps,
            "seed": self.seed,
            "reset": reset,
            "target_type": "dynamic" if target_func is not None else "static",
        }

        if metadata:
            base_metadata.update(metadata)

        self.run_metadata[run_key] = base_metadata

        return self

    def run_all(self, steps: int = 1000) -> "UIM_Ultimate_Diagnostics_Artifact":
        self.run(steps=steps, mode="A", reset=True, key="A")
        self.run(steps=steps, mode="B", reset=True, key="B")
        return self

    def run_conceptual_suite(
        self,
        steps: int = 1000,
        initial_policy: InitialPolicy = "same",
    ) -> "UIM_Ultimate_Diagnostics_Artifact":
        if not isinstance(steps, int) or isinstance(steps, bool):
            raise TypeError("steps must be an integer.")

        if steps < 1:
            raise ValueError("steps must be at least 1.")

        if initial_policy not in {"same", "different", "chain"}:
            raise ValueError("initial_policy must be 'same', 'different', or 'chain'.")

        for index, (name, meta) in enumerate(self.conceptual_mappings.items()):
            mode = meta["mode"]
            self._validate_mode(mode)

            if initial_policy == "same":
                self.reset()
                reset = False
                used_seed = self.seed
            elif initial_policy == "different":
                used_seed = self.seed + 10_000 + index
                self.nodes = self._generate_nodes(used_seed)
                reset = False
            else:
                used_seed = self.seed
                reset = index == 0

            self.run(
                steps=steps,
                mode=mode,
                reset=reset,
                key=name,
                metadata={
                    "kind": "conceptual_mapping",
                    "conceptual_label": name,
                    "description": meta["description"],
                    "note": meta["note"],
                    "initial_policy": initial_policy,
                    "used_seed": used_seed,
                },
            )

        return self

    def resolve_millennium_suite(
        self,
        steps: int = 1000,
        initial_policy: InitialPolicy = "same",
    ) -> "UIM_Ultimate_Diagnostics_Artifact":
        """
        Backward-compatible alias.

        This does not resolve the Millennium Prize Problems.
        It only runs the conceptual mapping suite.
        """
        print(
            "[Warning] resolve_millennium_suite() is a conceptual alias. "
            "No mathematical proof or resolution is performed."
        )
        return self.run_conceptual_suite(
            steps=steps,
            initial_policy=initial_policy,
        )

    # ------------------------------------------------------------------
    # Statistics
    # ------------------------------------------------------------------

    def get_statistics(self, key: str = "B") -> Dict[str, Any]:
        if key not in self.history:
            raise RuntimeError(f"Run key {key} has not been executed yet.")

        history = self.history[key]
        targets = self.target_logs[key]
        energy = self.energy_logs[key]
        meta = self.run_metadata.get(key, {})

        final_nodes = history[-1]
        final_target = targets[-1]
        final_mean = np.mean(final_nodes)
        final_deviation = final_nodes - final_target

        initial_target_energy = float(energy["target"][0])
        final_target_energy = float(energy["target"][-1])

        convergence_rate = (
            0.0
            if initial_target_energy == 0.0
            else (initial_target_energy - final_target_energy) / initial_target_energy
        )

        return {
            "key": key,
            "mode": self._mode_for_key(key),
            "target_type": meta.get("target_type", "unknown"),
            "initial_target_energy": initial_target_energy,
            "final_target_energy": final_target_energy,
            "initial_target_energy_mean": float(energy["target_mean"][0]),
            "final_target_energy_mean": float(energy["target_mean"][-1]),
            "convergence_rate": float(convergence_rate),
            "initial_physical_energy": float(energy["phys"][0]),
            "final_physical_energy": float(energy["phys"][-1]),
            "initial_consistency_energy": float(energy["cons"][0]),
            "final_consistency_energy": float(energy["cons"][-1]),
            "initial_coherence_energy": float(energy["coh"][0]),
            "final_coherence_energy": float(energy["coh"][-1]),
            "final_mean_real": float(final_mean.real),
            "final_mean_imag": float(final_mean.imag),
            "final_target_real": float(final_target.real),
            "final_target_imag": float(final_target.imag),
            "final_max_abs_deviation_from_target": float(
                np.max(np.abs(final_deviation))
            ),
            "final_mean_abs_deviation_from_target": float(
                np.mean(np.abs(final_deviation))
            ),
            "final_entropy_real": self.entropy_1d(final_nodes.real),
            "final_entropy_imag": self.entropy_1d(final_nodes.imag),
            "final_entropy_abs": self.entropy_1d(np.abs(final_nodes)),
            "final_entropy_phase_space": self.phase_space_entropy(final_nodes),
        }

    def energy_diagnostics(
        self,
        key: str = "B",
        tolerance: float = 1e-12,
    ) -> Dict[str, Any]:
        if key not in self.energy_logs:
            raise RuntimeError(f"Run key {key} has not been executed yet.")

        self._validate_non_negative_float("tolerance", tolerance)

        series = self.energy_logs[key]["target"]
        diff = np.diff(series)
        increases = diff > tolerance

        if np.any(increases):
            max_energy_increase = float(np.max(diff[increases]))
            first_increase_index = int(np.where(increases)[0][0] + 1)
        else:
            max_energy_increase = 0.0
            first_increase_index = -1

        return {
            "key": key,
            "is_monotonic": bool(np.all(~increases)),
            "num_energy_increases": int(np.sum(increases)),
            "max_energy_increase": max_energy_increase,
            "first_increase_index": first_increase_index,
            "initial_target_energy": float(series[0]),
            "final_target_energy": float(series[-1]),
            "min_target_energy": float(np.min(series)),
            "max_target_energy": float(np.max(series)),
        }

    def print_statistics(self, key: str = "B") -> "UIM_Ultimate_Diagnostics_Artifact":
        stats = self.get_statistics(key)
        diag = self.energy_diagnostics(key)
        meta = self.run_metadata.get(key, {})

        print(textwrap.dedent(f"""
        ================================================================================
        UIM Statistics - {key}
        --------------------------------------------------------------------------------
        Kind:
            {meta.get("kind", "unknown")}

        Conceptual Label:
            {meta.get("conceptual_label", key)}

        Note:
            {meta.get("note", "No note.")}

        Mode:
            {stats["mode"]}

        Target Type:
            {stats["target_type"]}

        Target Energy:
            initial      = {stats["initial_target_energy"]:.10f}
            final        = {stats["final_target_energy"]:.10e}
            final / node = {stats["final_target_energy_mean"]:.10e}

        Physical Energy:
            initial = {stats["initial_physical_energy"]:.10f}
            final   = {stats["final_physical_energy"]:.10e}

        Consistency Energy:
            initial = {stats["initial_consistency_energy"]:.10f}
            final   = {stats["final_consistency_energy"]:.10e}

        Coherence Energy:
            initial = {stats["initial_coherence_energy"]:.10f}
            final   = {stats["final_coherence_energy"]:.10e}

        Convergence Rate:
            {stats["convergence_rate"]:.10f}

        Final Mean State:
            {stats["final_mean_real"]:.10f} + {stats["final_mean_imag"]:.10f}j

        Final Target:
            {stats["final_target_real"]:.10f} + {stats["final_target_imag"]:.10f}j

        Final Deviation From Target:
            max  = {stats["final_max_abs_deviation_from_target"]:.10e}
            mean = {stats["final_mean_abs_deviation_from_target"]:.10e}

        Entropy:
            real component = {stats["final_entropy_real"]:.10f}
            imag component = {stats["final_entropy_imag"]:.10f}
            abs component  = {stats["final_entropy_abs"]:.10f}
            phase space    = {stats["final_entropy_phase_space"]:.10f}

        Energy Diagnostics:
            monotonic decrease   = {diag["is_monotonic"]}
            max energy increase  = {diag["max_energy_increase"]:.10e}
            number of increases  = {diag["num_energy_increases"]}
            first increase index = {diag["first_increase_index"]}
        ================================================================================
        """).strip())

        return self

    # ------------------------------------------------------------------
    # Suite summary / canonical report
    # ------------------------------------------------------------------

    def suite_table(self) -> list[Dict[str, Any]]:
        rows = []

        for name in self.conceptual_mappings:
            if name not in self.history:
                continue

            stats = self.get_statistics(name)
            meta = self.run_metadata.get(name, {})

            rows.append(
                {
                    "label": name,
                    "mode": stats["mode"],
                    "description": meta.get("description", ""),
                    "initial_target_energy": stats["initial_target_energy"],
                    "final_target_energy": stats["final_target_energy"],
                    "convergence_rate": stats["convergence_rate"],
                    "max_deviation": stats["final_max_abs_deviation_from_target"],
                    "phase_space_entropy": stats["final_entropy_phase_space"],
                    "note": meta.get("note", ""),
                }
            )

        return rows

    def print_suite_summary(self) -> "UIM_Ultimate_Diagnostics_Artifact":
        if not any(name in self.history for name in self.conceptual_mappings):
            raise RuntimeError("Conceptual suite has not been run yet.")

        print(textwrap.dedent("""
        ================================================================================
        UIM Conceptual Mapping Suite Summary
        --------------------------------------------------------------------------------
        Reminder:
            These are conceptual mappings, not mathematical proofs.
            Suite labels do not encode the full mathematical structure of each problem.
        ================================================================================
        """).strip())

        for row in self.suite_table():
            print(textwrap.dedent(f"""
            [{row["label"]}]
                Mode                 : {row["mode"]}
                Description          : {row["description"]}
                Initial Target       : {row["initial_target_energy"]:.10f}
                Final Target         : {row["final_target_energy"]:.10e}
                Convergence Rate     : {row["convergence_rate"]:.10f}
                Max Target Deviation : {row["max_deviation"]:.10e}
                Phase-Space Entropy  : {row["phase_space_entropy"]:.10f}
                Note                 : {row["note"]}
            """).rstrip())

        return self

    def canonical_report(self) -> str:
        if not any(name in self.history for name in self.conceptual_mappings):
            raise RuntimeError("Conceptual suite has not been run yet.")

        check = self.self_check()

        lines = [
            "================================================================================",
            f"UIM Unified Intelligence Core v{self.VERSION}",
            "Canonical Conceptual Simulation Record",
            "--------------------------------------------------------------------------------",
            "Boundary:",
            "  This is a reproducible simulation record, not a mathematical proof.",
            "  It does not solve or resolve any Millennium Prize Problem.",
            "",
            "Self-Check:",
            f"  Passed                         : {check['passed']}",
            f"  Deterministic Replay Error     : {check['deterministic_replay_error']:.10e}",
            f"  Mode A Fixed-Point Error       : {check['mode_a_fixed_point_error']:.10e}",
            f"  Mode B Fixed-Point Error       : {check['mode_b_fixed_point_error']:.10e}",
            "",
            "Conceptual Suite:",
        ]

        for row in self.suite_table():
            lines.extend(
                [
                    f"  [{row['label']}]",
                    f"    Mode                 : {row['mode']}",
                    f"    Description          : {row['description']}",
                    f"    Initial Target Energy: {row['initial_target_energy']:.10f}",
                    f"    Final Target Energy  : {row['final_target_energy']:.10e}",
                    f"    Convergence Rate     : {row['convergence_rate']:.10f}",
                    f"    Max Target Deviation : {row['max_deviation']:.10e}",
                    f"    Phase-Space Entropy  : {row['phase_space_entropy']:.10f}",
                    f"    Note                 : {row['note']}",
                    "",
                ]
            )

        lines.extend(
            [
                "Interpretation:",
                "  The values above describe convergence inside the designed UIM dynamical system.",
                "  They do not establish equivalence with the original mathematical problems.",
                "================================================================================",
            ]
        )

        return "\n".join(lines)

    def print_canonical_report(self) -> "UIM_Ultimate_Diagnostics_Artifact":
        print(self.canonical_report())
        return self

    # ------------------------------------------------------------------
    # Frequency analysis
    # ------------------------------------------------------------------

    def analyze_frequency(
        self,
        key: str = "B",
        node_index: Optional[int] = None,
        component: Component = "imag",
        start_ratio: float = 0.0,
        end_ratio: float = 1.0,
        remove_mean: bool = True,
        apply_window: bool = True,
        normalize: bool = True,
        min_peak_amplitude: float = 1e-10,
    ) -> Dict[str, Any]:
        if key not in self.history:
            raise RuntimeError(f"Run key {key} has not been executed yet.")

        if component not in {"real", "imag", "abs"}:
            raise ValueError("component must be 'real', 'imag', or 'abs'.")

        self._validate_ratio_interval(start_ratio, end_ratio)
        self._validate_non_negative_float("min_peak_amplitude", min_peak_amplitude)

        history = self.history[key]
        total_len = history.shape[0]
        start = int(total_len * float(start_ratio))
        end = int(total_len * float(end_ratio))

        if end - start < 4:
            raise ValueError("Selected interval is too short for FFT analysis.")

        segment = history[start:end]

        if node_index is None:
            complex_signal = np.mean(segment, axis=1)
            source_label = "field_mean"
        else:
            if not isinstance(node_index, int) or isinstance(node_index, bool):
                raise TypeError("node_index must be an integer or None.")
            if not (0 <= node_index < self.params["num_nodes"]):
                raise IndexError("node_index is out of range.")

            complex_signal = segment[:, node_index]
            source_label = f"node_{node_index}"

        if component == "real":
            raw_signal = complex_signal.real
        elif component == "imag":
            raw_signal = complex_signal.imag
        else:
            raw_signal = np.abs(complex_signal)

        raw_signal = np.asarray(raw_signal, dtype=float)
        signal = raw_signal.copy()

        if remove_mean:
            signal = signal - np.mean(signal)

        if apply_window:
            window = np.hanning(len(signal))
            signal_for_fft = signal * window
            window_scale = np.sum(window) / 2.0
        else:
            signal_for_fft = signal
            window_scale = len(signal) / 2.0

        freqs = np.fft.rfftfreq(len(signal_for_fft), d=self.params["dt"])
        spectrum = np.abs(np.fft.rfft(signal_for_fft))

        if normalize and window_scale > 0:
            spectrum = spectrum / window_scale

        if len(spectrum) > 1:
            candidate_index = 1 + int(np.argmax(spectrum[1:]))
            candidate_amplitude = float(spectrum[candidate_index])

            if candidate_amplitude > min_peak_amplitude:
                peak_index = candidate_index
                has_significant_peak = True
            else:
                peak_index = 0
                has_significant_peak = False
        else:
            peak_index = 0
            has_significant_peak = False

        peak_frequency = float(freqs[peak_index]) if has_significant_peak else 0.0
        peak_amplitude = float(spectrum[peak_index]) if has_significant_peak else 0.0

        result = {
            "key": key,
            "freqs": freqs,
            "spectrum": spectrum,
            "raw_signal": raw_signal,
            "signal": signal,
            "peak_frequency": peak_frequency,
            "peak_amplitude": peak_amplitude,
            "has_significant_peak": has_significant_peak,
            "source_label": source_label,
            "component": component,
            "start_ratio": float(start_ratio),
            "end_ratio": float(end_ratio),
        }

        log_key = (
            f"{key}_{source_label}_{component}_"
            f"{float(start_ratio):.4f}_{float(end_ratio):.4f}_"
            f"rm{int(remove_mean)}_win{int(apply_window)}_norm{int(normalize)}"
        )

        self.frequency_logs[log_key] = result

        return result

    def print_frequency_summary(
        self,
        key: str = "B",
        node_index: Optional[int] = None,
        component: Component = "imag",
        start_ratio: float = 0.0,
        end_ratio: float = 1.0,
        min_peak_amplitude: float = 1e-10,
    ) -> "UIM_Ultimate_Diagnostics_Artifact":
        result = self.analyze_frequency(
            key=key,
            node_index=node_index,
            component=component,
            start_ratio=start_ratio,
            end_ratio=end_ratio,
            min_peak_amplitude=min_peak_amplitude,
        )

        source = "field mean" if node_index is None else f"node {node_index}"

        print(textwrap.dedent(f"""
        ================================================================================
        UIM Frequency Summary - {key}
        --------------------------------------------------------------------------------
        Source               : {source}
        Component            : {component}
        Interval             : {start_ratio:.2f} - {end_ratio:.2f}
        Significant Peak     : {result["has_significant_peak"]}
        Peak Frequency:
            {result["peak_frequency"]:.10f}
        Peak Amplitude:
            {result["peak_amplitude"]:.10e}
        ================================================================================
        """).strip())

        return self

    # ------------------------------------------------------------------
    # Sensitivity sweep
    # ------------------------------------------------------------------

    def run_sensitivity_sweep(
        self,
        param: str,
        values: Iterable[float],
        steps: int = 500,
        mode: Mode = "B",
        metric: Literal[
            "final_target_energy",
            "convergence_rate",
            "phase_space_entropy",
        ] = "final_target_energy",
    ) -> list[Dict[str, Any]]:
        param_to_ctor = {
            "num_nodes": "num_nodes",
            "dt": "dt",
            "g": "g",
            "coupling": "coupling",
            "real_restore": "real_restore_rate",
            "imag_decay": "imag_decay_rate",
            "mode_a_decay": "mode_a_decay_rate",
            "mode_a_linear_decay": "mode_a_linear_decay_rate",
            "seed": "seed",
            "initial_real_span": "initial_real_span",
            "initial_imag_span": "initial_imag_span",
        }

        if param not in param_to_ctor:
            raise KeyError(f"Unknown sweep parameter: {param}")

        if not isinstance(steps, int) or isinstance(steps, bool):
            raise TypeError("steps must be an integer.")

        if steps < 1:
            raise ValueError("steps must be at least 1.")

        self._validate_mode(mode)

        results: list[Dict[str, Any]] = []

        for value in values:
            ctor_key = param_to_ctor[param]

            if param in {"num_nodes", "seed"}:
                override_value: Any = int(value)
            else:
                override_value = float(value)

            probe = self._spawn(overrides={ctor_key: override_value})
            probe.run(steps=steps, mode=mode, reset=True, key="probe")
            stats = probe.get_statistics("probe")

            if metric == "final_target_energy":
                metric_value = stats["final_target_energy"]
            elif metric == "convergence_rate":
                metric_value = stats["convergence_rate"]
            elif metric == "phase_space_entropy":
                metric_value = stats["final_entropy_phase_space"]
            else:
                raise ValueError(f"Unknown metric: {metric}")

            results.append(
                {
                    "param": param,
                    "value": override_value,
                    "mode": mode,
                    "metric": metric,
                    "metric_value": float(metric_value),
                    "final_target_energy": stats["final_target_energy"],
                    "convergence_rate": stats["convergence_rate"],
                    "phase_space_entropy": stats["final_entropy_phase_space"],
                }
            )

        return results

    # ------------------------------------------------------------------
    # Self-check / master report
    # ------------------------------------------------------------------

    def self_check(
        self,
        steps: int = 500,
        tolerance: float = 1e-8,
    ) -> Dict[str, Any]:
        if not isinstance(steps, int) or isinstance(steps, bool):
            raise TypeError("steps must be an integer.")

        if steps < 10:
            raise ValueError("steps must be at least 10.")

        self._validate_non_negative_float("tolerance", tolerance)

        origin_nodes = np.full(self.params["num_nodes"], self.origin, dtype=complex)

        target_nodes = np.full(
            self.params["num_nodes"],
            self.consistency_target,
            dtype=complex,
        )

        mode_a_next = self._step_mode_a(origin_nodes, self.origin)
        mode_b_next = self._step_mode_b(target_nodes, self.consistency_target)

        mode_a_fixed_error = float(np.max(np.abs(mode_a_next - origin_nodes)))
        mode_b_fixed_error = float(np.max(np.abs(mode_b_next - target_nodes)))

        probe_1 = self._spawn()
        probe_1.run(steps=steps, mode="B", reset=True, key="B")
        final_1 = probe_1.history["B"][-1]

        probe_2 = self._spawn()
        probe_2.run(steps=steps, mode="B", reset=True, key="B")
        final_2 = probe_2.history["B"][-1]

        deterministic_replay_error = float(np.max(np.abs(final_1 - final_2)))

        probe_a = self._spawn()
        probe_a.run(steps=steps, mode="A", reset=True, key="A")
        stats_a = probe_a.get_statistics("A")

        probe_b = self._spawn()
        probe_b.run(steps=steps, mode="B", reset=True, key="B")
        stats_b = probe_b.get_statistics("B")

        result = {
            "mode_a_fixed_point_error": mode_a_fixed_error,
            "mode_b_fixed_point_error": mode_b_fixed_error,
            "deterministic_replay_error": deterministic_replay_error,
            "mode_a_target_energy_reduced": bool(
                stats_a["final_target_energy"] < stats_a["initial_target_energy"]
            ),
            "mode_b_target_energy_reduced": bool(
                stats_b["final_target_energy"] < stats_b["initial_target_energy"]
            ),
            "mode_b_close_to_target": bool(
                stats_b["final_max_abs_deviation_from_target"] < 1e-2
            ),
        }

        result["passed"] = bool(
            result["mode_a_fixed_point_error"] <= tolerance
            and result["mode_b_fixed_point_error"] <= tolerance
            and result["deterministic_replay_error"] <= tolerance
            and result["mode_a_target_energy_reduced"]
            and result["mode_b_target_energy_reduced"]
            and result["mode_b_close_to_target"]
        )

        return result

    def master_report(self, steps: int = 500) -> "UIM_Ultimate_Diagnostics_Artifact":
        check = self.self_check(steps=steps)

        print(textwrap.dedent(f"""
        ================================================================================
        UIM Core v{self.VERSION} Master Report
        --------------------------------------------------------------------------------
        Integrity Check Passed:
            {check["passed"]}

        Fixed Point Errors:
            Mode A target error = {check["mode_a_fixed_point_error"]:.10e}
            Mode B target error = {check["mode_b_fixed_point_error"]:.10e}

        Reproducibility:
            deterministic replay error = {check["deterministic_replay_error"]:.10e}

        Energy Reduction:
            Mode A reduced = {check["mode_a_target_energy_reduced"]}
            Mode B reduced = {check["mode_b_target_energy_reduced"]}

        Target Proximity:
            Mode B close to target = {check["mode_b_close_to_target"]}

        Boundary:
            Conceptual diagnostics only.
            This is not a proof engine.
        ================================================================================
        """).strip())

        return self

    # ------------------------------------------------------------------
    # Visualization
    # ------------------------------------------------------------------

    @staticmethod
    def _selected_indices(num_nodes: int, count: int) -> np.ndarray:
        count = min(count, num_nodes)
        return np.unique(np.linspace(0, num_nodes - 1, count, dtype=int))

    def visualize(
        self,
        key: str = "B",
        max_nodes_to_plot: int = 20,
        fft_node_index: Optional[int] = None,
        fft_component: Component = "imag",
        fft_start_ratio: float = 0.0,
        fft_end_ratio: float = 1.0,
        min_peak_amplitude: float = 1e-10,
    ) -> "UIM_Ultimate_Diagnostics_Artifact":
        if key not in self.history:
            raise RuntimeError(f"Run key {key} has not been executed yet.")

        if not isinstance(max_nodes_to_plot, int) or isinstance(max_nodes_to_plot, bool):
            raise TypeError("max_nodes_to_plot must be an integer.")

        if max_nodes_to_plot <= 0:
            raise ValueError("max_nodes_to_plot must be positive.")

        history = self.history[key]
        targets = self.target_logs[key]
        energy = self.energy_logs[key]
        mode = self._mode_for_key(key)

        selected = self._selected_indices(
            self.params["num_nodes"],
            max_nodes_to_plot,
        )

        fft_result = self.analyze_frequency(
            key=key,
            node_index=fft_node_index,
            component=fft_component,
            start_ratio=fft_start_ratio,
            end_ratio=fft_end_ratio,
            min_peak_amplitude=min_peak_amplitude,
        )

        final_target = targets[-1]
        eps = 1e-16
        meta = self.run_metadata.get(key, {})
        note = meta.get("note", "")

        with plt.style.context("dark_background"):
            fig = plt.figure(figsize=(24, 5))

            ax1 = fig.add_subplot(151)
            for index in selected:
                ax1.plot(history[:, index].real, history[:, index].imag, alpha=0.35)

            ax1.plot(
                targets.real,
                targets.imag,
                color="orange",
                linestyle="--",
                linewidth=1.5,
                label="target path",
            )
            ax1.scatter(
                [final_target.real],
                [final_target.imag],
                marker="x",
                s=100,
                color="white",
                label="final target",
            )
            ax1.set_title(f"{key}: Complex Attractor | Mode {mode}", color="cyan")
            ax1.set_xlabel("Real")
            ax1.set_ylabel("Imaginary")
            ax1.grid(True, linestyle=":", alpha=0.3)
            ax1.legend()

            ax2 = fig.add_subplot(152)
            ax2.plot(history[:, selected].real, alpha=0.25)
            ax2.plot(targets.real, color="red", linestyle="--", linewidth=2)
            ax2.set_title("Real Part Dynamics", color="lime")
            ax2.set_xlabel("Step")
            ax2.set_ylabel("Real Part")
            ax2.grid(True, linestyle=":", alpha=0.3)

            ax3 = fig.add_subplot(153)
            ax3.plot(history[:, selected].imag, alpha=0.25)
            ax3.plot(targets.imag, color="red", linestyle="--", linewidth=2)
            ax3.set_title("Imaginary Part Dynamics", color="magenta")
            ax3.set_xlabel("Step")
            ax3.set_ylabel("Imaginary Part")
            ax3.grid(True, linestyle=":", alpha=0.3)

            ax4 = fig.add_subplot(154)
            ax4.plot(fft_result["freqs"], fft_result["spectrum"], color="magenta")
            ax4.set_title("Transient Spectrum rFFT", color="magenta")
            ax4.set_xlabel("Frequency")
            ax4.set_ylabel("Amplitude")
            ax4.grid(True, linestyle=":", alpha=0.3)
            ax4.set_xlim(left=0)

            if fft_result["has_significant_peak"]:
                ax4.scatter(
                    [fft_result["peak_frequency"]],
                    [fft_result["peak_amplitude"]],
                    color="white",
                    marker="x",
                    s=80,
                    label="significant peak",
                )
                ax4.legend()

            ax5 = fig.add_subplot(155)
            ax5.semilogy(np.maximum(energy["target"], eps), label="Target")
            ax5.semilogy(np.maximum(energy["phys"], eps), label="Physical")
            ax5.semilogy(np.maximum(energy["cons"], eps), label="Consistency")
            ax5.semilogy(np.maximum(energy["coh"], eps), label="Coherence")
            ax5.set_title("Energy Analysis", color="yellow")
            ax5.set_xlabel("Step")
            ax5.set_ylabel("Log Energy")
            ax5.grid(True, linestyle=":", alpha=0.3)
            ax5.legend()

            if note:
                fig.suptitle(note, fontsize=10, y=1.03)

            plt.tight_layout()
            plt.show()

        return self

    def visualize_suite(
        self,
        max_nodes_to_plot: int = 12,
        cols: int = 3,
    ) -> "UIM_Ultimate_Diagnostics_Artifact":
        suite_keys = [
            name for name in self.conceptual_mappings
            if name in self.history
        ]

        if not suite_keys:
            raise RuntimeError("Conceptual suite has not been run yet.")

        if not isinstance(max_nodes_to_plot, int) or isinstance(max_nodes_to_plot, bool):
            raise TypeError("max_nodes_to_plot must be an integer.")

        if max_nodes_to_plot <= 0:
            raise ValueError("max_nodes_to_plot must be positive.")

        if not isinstance(cols, int) or isinstance(cols, bool):
            raise TypeError("cols must be an integer.")

        if cols <= 0:
            raise ValueError("cols must be positive.")

        rows = math.ceil(len(suite_keys) / cols)

        with plt.style.context("dark_background"):
            fig, axes = plt.subplots(
                rows,
                cols,
                figsize=(6 * cols, 5 * rows),
                squeeze=False,
            )

            axes_flat = list(axes.flat)

            for ax, key in zip(axes_flat, suite_keys):
                history = self.history[key]
                targets = self.target_logs[key]
                stats = self.get_statistics(key)
                mode = self._mode_for_key(key)

                selected = self._selected_indices(
                    self.params["num_nodes"],
                    max_nodes_to_plot,
                )

                for index in selected:
                    ax.plot(history[:, index].real, history[:, index].imag, alpha=0.25)

                ax.plot(
                    targets.real,
                    targets.imag,
                    color="orange",
                    linestyle="--",
                    linewidth=1.5,
                )

                ax.scatter(
                    [targets[-1].real],
                    [targets[-1].imag],
                    marker="x",
                    s=80,
                    color="white",
                )

                ax.set_title(
                    f"{key} | Mode {mode}\n"
                    f"Final target energy={stats['final_target_energy']:.2e}",
                    color="cyan",
                )

                ax.set_xlabel("Real")
                ax.set_ylabel("Imaginary")
                ax.grid(True, linestyle=":", alpha=0.3)

            for ax in axes_flat[len(suite_keys):]:
                ax.axis("off")

            fig.suptitle(
                "UIM Conceptual Mapping Suite\n"
                "Canonical simulation record only; not mathematical proof.",
                fontsize=14,
                y=1.02,
            )

            plt.tight_layout()
            plt.show()

        return self

    def report_and_visualize(self) -> "UIM_Ultimate_Diagnostics_Artifact":
        if not any(name in self.history for name in self.conceptual_mappings):
            self.run_conceptual_suite(steps=1000, initial_policy="same")

        self.print_canonical_report()
        self.visualize_suite()

        return self


UIMUnifiedObject = UIM_Ultimate_Diagnostics_Artifact


if __name__ == "__main__":
    artifact = UIM_Ultimate_Diagnostics_Artifact(
        num_nodes=100,
        seed=42,
    ).boot()

    artifact.master_report()

    artifact.run_conceptual_suite(
        steps=1000,
        initial_policy="same",
    )

    artifact.print_canonical_report()
    artifact.visualize_suite()

    # Dynamic target demo
    def moving_target(step: int) -> complex:
        t = step * 0.01
        return 0.5 + 0.1j * np.sin(t)

    artifact.run(
        steps=1000,
        mode="B",
        key="Dynamic_Target_Demo",
        reset=True,
        target_func=moving_target,
        metadata={
            "kind": "dynamic_target_demo",
            "note": "Dynamic target tracking demo; not a theorem.",
        },
    )

    artifact.print_statistics("Dynamic_Target_Demo")
    artifact.visualize("Dynamic_Target_Demo")

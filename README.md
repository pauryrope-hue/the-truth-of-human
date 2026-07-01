from __future__ import annotations

import textwrap
from typing import Any, Callable, Dict, Iterable, Literal, Optional

import numpy as np


Mode = Literal["A", "B"]


class UIMUnifiedObject:
    """
    UIM Unified Intelligence Core v7.5
    Ultimate Integrated Millennium Diagnostics Artifact

    Boundary:
        This is a conceptual simulation artifact, not a mathematical proof engine.
        It does not prove, solve, or resolve any Millennium Prize Problem.

    Purpose:
        This single object runs UIM-style conceptual diagnostics for six
        Millennium Prize Problem labels by mapping them onto designed dynamical
        modes.

        P vs NP        -> Mode A: dissipative convergence toward 0+0j
        Riemann        -> Mode B: convergence toward 0.5+0j
        Navier-Stokes  -> Mode B: relaxation analogy
        Yang-Mills     -> Mode B: stabilization analogy
        BSD            -> Mode B: coherence alignment analogy
        Hodge          -> Mode B: topological consistency analogy

    Important:
        Multiple labels sharing the same mode, target, parameters, and initial
        condition will produce identical numerical results. That is expected.
        The labels are conceptual interpretations, not encoded mathematical
        structures of the original problems.
    """

    VERSION = "7.5-refined"
    VALID_MODES = {"A", "B"}

    def __init__(
        self,
        num_nodes: int = 500,
        seed: int = 42,
        dt: float = 0.01,
        g: float = 2.5,
        coupling: float = 0.1,
        real_restore_rate: float = 2.5,
        imag_decay_rate: float = 1.0,
        mode_a_decay_rate: float = 0.1 * np.exp(5.0),
        mode_a_linear_decay_rate: float = 0.05,
        initial_real_span: float = 1.5,
        initial_imag_span: float = 3.0,
    ) -> None:
        self._validate_positive_int("num_nodes", num_nodes)
        self._validate_integer("seed", seed)
        self._validate_positive_float("dt", dt)
        self._validate_non_negative_float("g", g)
        self._validate_non_negative_float("coupling", coupling)
        self._validate_non_negative_float("real_restore_rate", real_restore_rate)
        self._validate_non_negative_float("imag_decay_rate", imag_decay_rate)
        self._validate_non_negative_float("mode_a_decay_rate", mode_a_decay_rate)
        self._validate_non_negative_float(
            "mode_a_linear_decay_rate",
            mode_a_linear_decay_rate,
        )
        self._validate_positive_float("initial_real_span", initial_real_span)
        self._validate_positive_float("initial_imag_span", initial_imag_span)

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
        self.run_metadata: Dict[str, Dict[str, Any]] = {}

        self.master_report_data: Dict[str, Dict[str, Any]] = {}
        self.model_verification_results: Dict[str, Dict[str, Any]] = {}
        self.stability_boundary_results: Dict[str, Dict[str, Any]] = {}

        self.problems = self._build_problem_suite()

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
    def _validate_mode(cls, mode: str) -> None:
        if mode not in cls.VALID_MODES:
            raise ValueError("mode must be 'A' or 'B'.")

    @staticmethod
    def _validate_bins(bins: int) -> None:
        if not isinstance(bins, int) or isinstance(bins, bool):
            raise TypeError("bins must be an integer.")
        if bins <= 1:
            raise ValueError("bins must be greater than 1.")

    # ------------------------------------------------------------------
    # Suite / initialization
    # ------------------------------------------------------------------

    @staticmethod
    def _build_problem_suite() -> Dict[str, Dict[str, str]]:
        return {
            "P_vs_NP": {
                "mode": "A",
                "description": "metaphorical dissipation of inconsistent paths",
                "note": "Conceptual Mode A diagnostic only; not a proof of P vs NP.",
            },
            "Riemann": {
                "mode": "B",
                "description": "visual attraction toward Re = 0.5",
                "note": "Conceptual Mode B diagnostic only; not a proof of the Riemann Hypothesis.",
            },
            "Navier_Stokes": {
                "mode": "B",
                "description": "relaxation of informational congestion",
                "note": "Conceptual Mode B diagnostic only; not a proof of Navier-Stokes regularity.",
            },
            "Yang_Mills": {
                "mode": "B",
                "description": "structural stabilization analogy",
                "note": "Conceptual Mode B diagnostic only; not a proof of Yang-Mills mass gap.",
            },
            "BSD": {
                "mode": "B",
                "description": "simplified coherence alignment",
                "note": "Conceptual Mode B diagnostic only; not a proof of Birch and Swinnerton-Dyer.",
            },
            "Hodge": {
                "mode": "B",
                "description": "topological consistency alignment analogy",
                "note": "Conceptual Mode B diagnostic only; not a proof of the Hodge conjecture.",
            },
        }

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

    def reset(self) -> "UIMUnifiedObject":
        self.nodes = self.initial_nodes.copy()
        return self

    def reseed(self, seed: int) -> "UIMUnifiedObject":
        self._validate_integer("seed", seed)
        self.seed = int(seed)
        self.initial_nodes = self._generate_nodes(self.seed)
        self.reset()
        return self

    def default_target(self, mode: Mode) -> complex:
        self._validate_mode(mode)
        return self.origin if mode == "A" else self.consistency_target

    # ------------------------------------------------------------------
    # Energy / entropy
    # ------------------------------------------------------------------

    @staticmethod
    def target_energy(nodes: np.ndarray, target: complex) -> float:
        return float(np.sum(np.abs(nodes - target) ** 2))

    @staticmethod
    def physical_energy(nodes: np.ndarray) -> float:
        return float(np.sum(np.abs(nodes) ** 2))

    def consistency_energy(self, nodes: np.ndarray) -> float:
        return float(np.sum(np.abs(nodes - self.consistency_target) ** 2))

    @staticmethod
    def coherence_energy(nodes: np.ndarray) -> float:
        mean_state = np.mean(nodes)
        return float(np.sum(np.abs(nodes - mean_state) ** 2))

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
    def fixed_range_phase_entropy(
        cls,
        nodes: np.ndarray,
        bins: int = 20,
        real_range: tuple[float, float] = (-1.0, 1.0),
        imag_range: tuple[float, float] = (-1.5, 1.5),
    ) -> float:
        cls._validate_bins(bins)

        counts, _, _ = np.histogram2d(
            nodes.real,
            nodes.imag,
            bins=bins,
            range=[real_range, imag_range],
            density=False,
        )

        total = np.sum(counts)

        if total == 0:
            return 0.0

        probs = counts.ravel().astype(float) / float(total)
        probs = probs[probs > 0]

        return float(-np.sum(probs * np.log(probs)))

    def _new_energy_log(self, steps: int) -> Dict[str, np.ndarray]:
        return {
            "target": np.zeros(steps + 1),
            "physical": np.zeros(steps + 1),
            "consistency": np.zeros(steps + 1),
            "coherence": np.zeros(steps + 1),
        }

    def _record_energy(
        self,
        energy_log: Dict[str, np.ndarray],
        index: int,
        nodes: np.ndarray,
        target: complex,
    ) -> None:
        energy_log["target"][index] = self.target_energy(nodes, target)
        energy_log["physical"][index] = self.physical_energy(nodes)
        energy_log["consistency"][index] = self.consistency_energy(nodes)
        energy_log["coherence"][index] = self.coherence_energy(nodes)

    # ------------------------------------------------------------------
    # UIM kernel
    # ------------------------------------------------------------------

    def _apply_rotation_about(
        self,
        nodes: np.ndarray,
        center: complex,
        *,
        g_override: Optional[float] = None,
    ) -> np.ndarray:
        g = self.params["g"] if g_override is None else float(g_override)

        dev = nodes - center
        intensity = np.abs(dev) ** 2
        phase = g * intensity * self.params["dt"]

        return center + dev * np.exp(1j * phase)

    def _step_mode_a(
        self,
        nodes: np.ndarray,
        target: complex,
        *,
        g_override: Optional[float] = None,
    ) -> np.ndarray:
        rotated = self._apply_rotation_about(
            nodes,
            target,
            g_override=g_override,
        )

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
        *,
        g_override: Optional[float] = None,
    ) -> np.ndarray:
        rotated = self._apply_rotation_about(
            nodes,
            target,
            g_override=g_override,
        )

        coupled = rotated + self.params["coupling"] * (
            np.mean(rotated) - rotated
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
        *,
        g_override: Optional[float] = None,
    ) -> np.ndarray:
        self._validate_mode(mode)

        if mode == "A":
            return self._step_mode_a(
                nodes,
                target,
                g_override=g_override,
            )

        return self._step_mode_b(
            nodes,
            target,
            g_override=g_override,
        )

    # ------------------------------------------------------------------
    # Execution
    # ------------------------------------------------------------------

    def run_kernel(
        self,
        mode: Mode,
        *,
        steps: int = 1500,
        seed: Optional[int] = None,
        key: Optional[str] = None,
        reset: bool = True,
        target_func: Optional[Callable[[int], complex]] = None,
        g_override: Optional[float] = None,
        metadata: Optional[Dict[str, Any]] = None,
    ) -> np.ndarray:
        self._validate_mode(mode)
        self._validate_positive_int("steps", steps)

        if seed is None:
            seed = self.seed

        if reset:
            nodes = self._generate_nodes(seed)
        else:
            nodes = self.nodes.copy()

        n = self.params["num_nodes"]

        history = np.zeros((steps + 1, n), dtype=complex)
        target_log = np.zeros(steps + 1, dtype=complex)
        energy_log = self._new_energy_log(steps)

        def active_target(step_index: int) -> complex:
            if target_func is None:
                return self.default_target(mode)
            return complex(target_func(step_index))

        target_0 = active_target(0)

        history[0] = nodes.copy()
        target_log[0] = target_0
        self._record_energy(energy_log, 0, nodes, target_0)

        for step_index in range(1, steps + 1):
            target = active_target(step_index)

            nodes = self._next_nodes(
                nodes,
                mode,
                target,
                g_override=g_override,
            )

            history[step_index] = nodes.copy()
            target_log[step_index] = target
            self._record_energy(energy_log, step_index, nodes, target)

            if (
                not np.all(np.isfinite(nodes.real))
                or not np.all(np.isfinite(nodes.imag))
            ):
                break

        self.nodes = nodes.copy()

        if key is not None:
            self.history[key] = history
            self.target_logs[key] = target_log
            self.energy_logs[key] = energy_log

            self.run_metadata[key] = {
                "key": key,
                "mode": mode,
                "steps": steps,
                "seed": seed,
                "target_type": "dynamic" if target_func is not None else "static",
                "g_override": g_override,
                **(metadata or {}),
            }

        return nodes

    def get_statistics(self, key: str) -> Dict[str, Any]:
        if key not in self.history:
            raise KeyError(f"No run found for key: {key}")

        history = self.history[key]
        targets = self.target_logs[key]
        energy = self.energy_logs[key]
        meta = self.run_metadata.get(key, {})

        final_nodes = history[-1]
        final_target = targets[-1]

        initial_energy = float(energy["target"][0])
        final_energy = float(energy["target"][-1])

        convergence_rate = (
            0.0
            if initial_energy == 0.0
            else (initial_energy - final_energy) / initial_energy
        )

        diffs = np.diff(energy["target"])
        increases = diffs > 1e-12
        final_deviation = final_nodes - final_target

        return {
            "label": key,
            "mode": meta.get("mode", "unknown"),
            "description": meta.get("description", ""),
            "initial_target_energy": initial_energy,
            "final_target_energy": final_energy,
            "convergence_rate": float(convergence_rate),
            "max_abs_deviation": float(np.max(np.abs(final_deviation))),
            "mean_abs_deviation": float(np.mean(np.abs(final_deviation))),
            "final_mean_real": float(np.mean(final_nodes).real),
            "final_mean_imag": float(np.mean(final_nodes).imag),
            "final_entropy_real": self.entropy_1d(final_nodes.real),
            "final_entropy_imag": self.entropy_1d(final_nodes.imag),
            "final_entropy_abs": self.entropy_1d(np.abs(final_nodes)),
            "final_fixed_range_phase_entropy": self.fixed_range_phase_entropy(
                final_nodes
            ),
            "monotonic_target_energy": bool(np.sum(increases) == 0),
            "num_energy_increases": int(np.sum(increases)),
            "status": self._classify_status(final_energy, convergence_rate),
            "note": meta.get("note", ""),
        }

    @staticmethod
    def _classify_status(final_energy: float, convergence_rate: float) -> str:
        if final_energy < 1e-10:
            return "CONVERGED_WITHIN_MODEL"
        if final_energy < 1e-5:
            return "NEAR_CONVERGED_WITHIN_MODEL"
        if convergence_rate > 0.99:
            return "STRONGLY_DISSIPATED_WITHIN_MODEL"
        if convergence_rate > 0.5:
            return "PARTIALLY_DISSIPATED_WITHIN_MODEL"
        return "NOT_CONVERGED_WITHIN_MODEL"

    # ------------------------------------------------------------------
    # Full diagnostics
    # ------------------------------------------------------------------

    def run_full_system_analysis(
        self,
        steps: int = 1500,
        *,
        initial_policy: Literal["same", "different"] = "same",
    ) -> "UIMUnifiedObject":
        """
        Run all conceptual diagnostics.

        This method does not perform formal proof verification.
        It stores model-internal convergence diagnostics only.
        """
        self._validate_positive_int("steps", steps)

        if initial_policy not in {"same", "different"}:
            raise ValueError("initial_policy must be 'same' or 'different'.")

        self.master_report_data.clear()
        self.model_verification_results.clear()
        self.stability_boundary_results.clear()

        for index, (name, meta) in enumerate(self.problems.items()):
            mode: Mode = meta["mode"]  # type: ignore[assignment]

            seed = (
                self.seed
                if initial_policy == "same"
                else self.seed + 10000 + index
            )

            self.run_kernel(
                mode,
                steps=steps,
                seed=seed,
                key=name,
                reset=True,
                metadata={
                    "description": meta["description"],
                    "note": meta["note"],
                    "initial_policy": initial_policy,
                },
            )

            stats = self.get_statistics(name)
            self.master_report_data[name] = stats

            self.model_verification_results[name] = {
                "model_status": stats["status"],
                "verified_claim": "model convergence only",
                "formal_proof": False,
                "message": (
                    "Diagnostic convergence inside the UIM model; "
                    "not a mathematical proof."
                ),
            }

            self.stability_boundary_results[name] = self.find_stability_boundary(
                mode,
                steps=max(300, steps // 2),
                seed=seed,
            )

        return self

    def find_stability_boundary(
        self,
        mode: Mode,
        *,
        steps: int = 750,
        seed: Optional[int] = None,
        g_values: Optional[Iterable[float]] = None,
        divergence_norm: float = 1e10,
    ) -> Dict[str, Any]:
        """
        Sweep g without mutating self.params.

        The current UIM kernel is mostly contractive because Mode A and Mode B
        include dissipative/restorative factors. Therefore divergence may not be
        observed even for large g values. In that case, this method reports
        no divergence detected.
        """
        self._validate_mode(mode)
        self._validate_positive_int("steps", steps)
        self._validate_positive_float("divergence_norm", divergence_norm)

        if seed is None:
            seed = self.seed

        if g_values is None:
            g_values = np.linspace(2.5, 200.0, 20)

        target = self.default_target(mode)

        first_divergent_g: Optional[float] = None
        max_norm_seen = 0.0
        last_final_energy = 0.0

        for g in g_values:
            g_float = float(g)
            nodes = self._generate_nodes(seed)

            diverged = False

            for _ in range(steps):
                nodes = self._next_nodes(
                    nodes,
                    mode,
                    target,
                    g_override=g_float,
                )

                norm = float(np.sum(np.abs(nodes)))
                max_norm_seen = max(max_norm_seen, norm)

                if (
                    not np.all(np.isfinite(nodes.real))
                    or not np.all(np.isfinite(nodes.imag))
                    or norm > divergence_norm
                ):
                    diverged = True
                    break

            last_final_energy = self.target_energy(nodes, target)

            if diverged:
                first_divergent_g = g_float
                break

        return {
            "mode": mode,
            "first_divergent_g": first_divergent_g,
            "divergence_detected": first_divergent_g is not None,
            "max_norm_seen": float(max_norm_seen),
            "last_final_energy": float(last_final_energy),
            "sweep_note": (
                "No divergence detected in tested g range."
                if first_divergent_g is None
                else "Divergence detected under tested g sweep."
            ),
        }

    # ------------------------------------------------------------------
    # Self-check / reporting
    # ------------------------------------------------------------------

    def self_check(
        self,
        steps: int = 1000,
        tolerance: float = 1e-8,
    ) -> Dict[str, Any]:
        self._validate_positive_int("steps", steps)
        self._validate_non_negative_float("tolerance", tolerance)

        origin_nodes = np.full(
            self.params["num_nodes"],
            self.origin,
            dtype=complex,
        )

        target_nodes = np.full(
            self.params["num_nodes"],
            self.consistency_target,
            dtype=complex,
        )

        mode_a_fixed_error = float(
            np.max(np.abs(self._step_mode_a(origin_nodes, self.origin) - origin_nodes))
        )

        mode_b_fixed_error = float(
            np.max(
                np.abs(
                    self._step_mode_b(target_nodes, self.consistency_target)
                    - target_nodes
                )
            )
        )

        probe_1 = UIMUnifiedObject(
            num_nodes=self.params["num_nodes"],
            seed=self.seed,
            dt=self.params["dt"],
            g=self.params["g"],
            coupling=self.params["coupling"],
            real_restore_rate=self.params["real_restore"],
            imag_decay_rate=self.params["imag_decay"],
            mode_a_decay_rate=self.params["mode_a_decay"],
            mode_a_linear_decay_rate=self.params["mode_a_linear_decay"],
            initial_real_span=self.params["initial_real_span"],
            initial_imag_span=self.params["initial_imag_span"],
        )

        probe_2 = UIMUnifiedObject(
            num_nodes=self.params["num_nodes"],
            seed=self.seed,
            dt=self.params["dt"],
            g=self.params["g"],
            coupling=self.params["coupling"],
            real_restore_rate=self.params["real_restore"],
            imag_decay_rate=self.params["imag_decay"],
            mode_a_decay_rate=self.params["mode_a_decay"],
            mode_a_linear_decay_rate=self.params["mode_a_linear_decay"],
            initial_real_span=self.params["initial_real_span"],
            initial_imag_span=self.params["initial_imag_span"],
        )

        final_1 = probe_1.run_kernel(
            "B",
            steps=steps,
            key="check",
            reset=True,
        )

        final_2 = probe_2.run_kernel(
            "B",
            steps=steps,
            key="check",
            reset=True,
        )

        deterministic_replay_error = float(np.max(np.abs(final_1 - final_2)))

        return {
            "passed": bool(
                mode_a_fixed_error <= tolerance
                and mode_b_fixed_error <= tolerance
                and deterministic_replay_error <= tolerance
            ),
            "mode_a_fixed_point_error": mode_a_fixed_error,
            "mode_b_fixed_point_error": mode_b_fixed_error,
            "deterministic_replay_error": deterministic_replay_error,
        }

    def get_master_report(self) -> str:
        if not self.master_report_data:
            return "No diagnostic data available. Run run_full_system_analysis() first."

        check = self.self_check()

        report = [
            "================================================================================",
            f"UIM Unified Intelligence Core v{self.VERSION}",
            "Ultimate Integrated Millennium Diagnostics Report",
            "--------------------------------------------------------------------------------",
            "Boundary:",
            "  This is a conceptual UIM simulation report, not a mathematical proof.",
            "  No equivalence theorem is established between UIM convergence and the original problems.",
            "  Therefore, these results are model diagnostics, not mathematical solutions.",
            "",
            "Self-Check:",
            f"  Passed                     : {check['passed']}",
            f"  Mode A Fixed-Point Error   : {check['mode_a_fixed_point_error']:.10e}",
            f"  Mode B Fixed-Point Error   : {check['mode_b_fixed_point_error']:.10e}",
            f"  Deterministic Replay Error : {check['deterministic_replay_error']:.10e}",
            "",
            "Problem Labels:",
        ]

        for name in self.problems:
            data = self.master_report_data[name]
            verification = self.model_verification_results[name]
            boundary = self.stability_boundary_results[name]

            report.extend(
                [
                    f"  [{name}]",
                    f"    Mode                         : {data['mode']}",
                    f"    Description                  : {data['description']}",
                    f"    Status                       : {data['status']}",
                    f"    Initial Target Energy        : {data['initial_target_energy']:.10f}",
                    f"    Final Target Energy          : {data['final_target_energy']:.10e}",
                    f"    Convergence Rate             : {data['convergence_rate']:.10f}",
                    f"    Max Abs Deviation            : {data['max_abs_deviation']:.10e}",
                    f"    Fixed-Range Phase Entropy    : {data['final_fixed_range_phase_entropy']:.10f}",
                    f"    Energy Monotonic             : {data['monotonic_target_energy']}",
                    f"    Model Verification           : {verification['model_status']}",
                    f"    Formal Proof                 : {verification['formal_proof']}",
                    f"    Divergence Detected          : {boundary['divergence_detected']}",
                    f"    First Divergent g            : {boundary['first_divergent_g']}",
                    f"    Boundary Note                : {boundary['sweep_note']}",
                    f"    Note                         : {data['note']}",
                    "",
                ]
            )

        report.extend(
            [
                "Interpretation:",
                "  Mode A is asymptotically dissipative; finite-step residual energy may remain.",
                "  Mode B labels share the same underlying dynamics unless separate targets or parameters are assigned.",
                "  Stability-boundary sweeps inspect this designed kernel only and do not search for mathematical counterexamples.",
                "================================================================================",
            ]
        )

        return "\n".join(report)


if __name__ == "__main__":
    uim = UIMUnifiedObject()
    uim.run_full_system_analysis()
    print(uim.get_master_report())

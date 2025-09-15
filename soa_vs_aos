use std::time::Instant;

// Array of Structures (AoS) approach
#[derive(Debug, Clone)]
pub struct Point {
    pub x: f64,
    pub y: f64,
    pub heading: f64,
}

// Structure of Arrays (SoA) approach
#[derive(Debug, Clone)]
pub struct PointsSoA {
    pub x_values: Vec<f64>,
    pub y_values: Vec<f64>,
    pub heading_values: Vec<f64>,
}

impl PointsSoA {
    pub fn new(capacity: usize) -> Self {
        Self {
            x_values: Vec::with_capacity(capacity),
            y_values: Vec::with_capacity(capacity),
            heading_values: Vec::with_capacity(capacity),
        }
    }

    pub fn len(&self) -> usize {
        self.x_values.len()
    }

    pub fn push(&mut self, x: f64, y: f64, heading: f64) {
        self.x_values.push(x);
        self.y_values.push(y);
        self.heading_values.push(heading);
    }
}

pub mod setup {
    use super::{Point, PointsSoA};
    pub fn create_aos_test_data(num_points: usize) -> Vec<Point> {
        let mut points = Vec::with_capacity(num_points);
        for i in 0..num_points {
            points.push(Point {
                x: i as f64 * 10.0,
                y: i as f64 * 20.0,
                heading: (i as f64 * 30.0) % 360.0,
            });
        }
        points
    }
    pub fn create_soa_test_data(num_points: usize) -> PointsSoA {
        let mut points = PointsSoA::new(num_points);
        for i in 0..num_points {
            points.push(i as f64 * 10.0, i as f64 * 20.0, (i as f64 * 30.0) % 360.0);
        }
        points
    }
}

pub mod independent_operations {
    use super::setup;
    use super::{Point, PointsSoA};
    use std::time::Instant;

    pub fn perform_aos(points: &mut [Point]) {
        for point in points.iter_mut() {
            point.x += 1.0;
            point.y += 5.0;
            point.heading = (point.heading + 5.0) % 360.0;
        }
    }
    pub fn perform_soa(points: &mut PointsSoA) {
        for i in 0..points.len() {
            points.x_values[i] += 1.0;
            points.y_values[i] += 5.0;
            points.heading_values[i] = (points.heading_values[i] + 5.0) % 360.0;
        }
    }
    pub fn performance_test() {
        let test_sizes = [1000, 10000, 100000, 1000000];
        let iterations = 100;
        println!("Performance: Independent Operations");
        for &size in &test_sizes {
            println!("\n{} points, {} iterations:", size, iterations);
            let mut aos_data = setup::create_aos_test_data(size);
            let aos_start = Instant::now();
            for _ in 0..iterations {
                perform_aos(&mut aos_data);
            }
            let aos_duration = aos_start.elapsed();
            let mut soa_data = setup::create_soa_test_data(size);
            let soa_start = Instant::now();
            for _ in 0..iterations {
                perform_soa(&mut soa_data);
            }
            let soa_duration = soa_start.elapsed();
            println!("AoS: {:?} ({} ms)", aos_duration, aos_duration.as_millis());
            println!("SoA: {:?} ({} ms)", soa_duration, soa_duration.as_millis());
        }
    }
}

pub mod dependent_operations {
    use super::setup;
    use super::{Point, PointsSoA};
    use std::time::Instant;

    pub fn perform_aos(points: &mut [Point]) {
        for point in points.iter_mut() {
            let h = point.heading;
            if h >= 0.0 && h < 90.0 {
                point.x += 1.0;
                point.y += 1.0;
            } else if h >= 90.0 && h < 180.0 {
                point.x += 1.0;
                point.y -= 1.0;
            } else if h >= 180.0 && h < 270.0 {
                point.x -= 1.0;
                point.y -= 1.0;
            } else {
                point.x -= 1.0;
                point.y += 1.0;
            }
            point.heading = (point.heading + 2.0) % 360.0;
        }
    }
    pub fn perform_soa(points: &mut PointsSoA) {
        for i in 0..points.len() {
            let h = points.heading_values[i];
            if h >= 0.0 && h < 90.0 {
                points.x_values[i] += 1.0;
                points.y_values[i] += 1.0;
            } else if h >= 90.0 && h < 180.0 {
                points.x_values[i] += 1.0;
                points.y_values[i] -= 1.0;
            } else if h >= 180.0 && h < 270.0 {
                points.x_values[i] -= 1.0;
                points.y_values[i] -= 1.0;
            } else {
                points.x_values[i] -= 1.0;
                points.y_values[i] += 1.0;
            }
            points.heading_values[i] = (points.heading_values[i] + 2.0) % 360.0;
        }
    }
    pub fn performance_test() {
        let test_sizes = [1000, 10000, 100000, 1000000];
        let iterations = 100;
        println!("Performance: Dependent Operations");
        for &size in &test_sizes {
            println!("\n{} points, {} iterations:", size, iterations);
            let mut aos_data = setup::create_aos_test_data(size);
            let aos_start = Instant::now();
            for _ in 0..iterations {
                perform_aos(&mut aos_data);
            }
            let aos_duration = aos_start.elapsed();
            let mut soa_data = setup::create_soa_test_data(size);
            let soa_start = Instant::now();
            for _ in 0..iterations {
                perform_soa(&mut soa_data);
            }
            let soa_duration = soa_start.elapsed();
            println!("AoS: {:?} ({} ms)", aos_duration, aos_duration.as_millis());
            println!("SoA: {:?} ({} ms)", soa_duration, soa_duration.as_millis());
        }
    }
}

pub mod single_variable {
    use super::setup;
    use super::{Point, PointsSoA};
    use std::time::Instant;

    pub fn perform_aos(points: &mut [Point]) {
        for point in points.iter_mut() {
            point.x += 1.0;
        }
    }
    pub fn perform_soa(points: &mut PointsSoA) {
        for i in 0..points.len() {
            points.x_values[i] += 1.0;
        }
    }
    pub fn performance_test() {
        let test_sizes = [1000, 10000, 100000, 1000000];
        let iterations = 100;
        println!("Performance: Single Variable (x += 1)");
        for &size in &test_sizes {
            println!("\n{} points, {} iterations:", size, iterations);
            let mut aos_data = setup::create_aos_test_data(size);
            let aos_start = Instant::now();
            for _ in 0..iterations {
                perform_aos(&mut aos_data);
            }
            let aos_duration = aos_start.elapsed();
            let mut soa_data = setup::create_soa_test_data(size);
            let soa_start = Instant::now();
            for _ in 0..iterations {
                perform_soa(&mut soa_data);
            }
            let soa_duration = soa_start.elapsed();
            println!("AoS: {:?} ({} ms)", aos_duration, aos_duration.as_millis());
            println!("SoA: {:?} ({} ms)", soa_duration, soa_duration.as_millis());
        }
    }
}

#[cfg(test)]
mod tests {
    use super::dependent_operations;
    use super::independent_operations;
    use super::setup;
    use super::single_variable;
    use super::*;

    #[test]
    fn verify_independent_correctness() {
        let mut points_aos = setup::create_aos_test_data(10);
        let mut points_soa = setup::create_soa_test_data(10);
        let aos_original = points_aos.clone();
        let soa_original = PointsSoA {
            x_values: points_soa.x_values.clone(),
            y_values: points_soa.y_values.clone(),
            heading_values: points_soa.heading_values.clone(),
        };
        independent_operations::perform_aos(&mut points_aos);
        independent_operations::perform_soa(&mut points_soa);
        for i in 0..10 {
            let expected_x = aos_original[i].x + 1.0;
            let expected_y = aos_original[i].y + 5.0;
            let expected_heading = (aos_original[i].heading + 5.0) % 360.0;
            assert!((points_aos[i].x - expected_x).abs() < f64::EPSILON);
            assert!((points_aos[i].y - expected_y).abs() < f64::EPSILON);
            assert!((points_aos[i].heading - expected_heading).abs() < f64::EPSILON);
            assert!((points_soa.x_values[i] - expected_x).abs() < f64::EPSILON);
            assert!((points_soa.y_values[i] - expected_y).abs() < f64::EPSILON);
            assert!((points_soa.heading_values[i] - expected_heading).abs() < f64::EPSILON);
        }
    }

    #[test]
    fn verify_dependent_correctness() {
        let mut points_aos = setup::create_aos_test_data(10);
        let mut points_soa = setup::create_soa_test_data(10);
        let aos_original = points_aos.clone();
        let soa_original = PointsSoA {
            x_values: points_soa.x_values.clone(),
            y_values: points_soa.y_values.clone(),
            heading_values: points_soa.heading_values.clone(),
        };
        dependent_operations::perform_aos(&mut points_aos);
        dependent_operations::perform_soa(&mut points_soa);
        for i in 0..10 {
            let h = aos_original[i].heading;
            let (mut expected_x, mut expected_y) = (aos_original[i].x, aos_original[i].y);
            if h >= 0.0 && h < 90.0 {
                expected_x += 1.0;
                expected_y += 1.0;
            } else if h >= 90.0 && h < 180.0 {
                expected_x += 1.0;
                expected_y -= 1.0;
            } else if h >= 180.0 && h < 270.0 {
                expected_x -= 1.0;
                expected_y -= 1.0;
            } else {
                expected_x -= 1.0;
                expected_y += 1.0;
            }
            let expected_heading = (aos_original[i].heading + 2.0) % 360.0;
            assert!((points_aos[i].x - expected_x).abs() < f64::EPSILON);
            assert!((points_aos[i].y - expected_y).abs() < f64::EPSILON);
            assert!((points_aos[i].heading - expected_heading).abs() < f64::EPSILON);
            assert!((points_soa.x_values[i] - expected_x).abs() < f64::EPSILON);
            assert!((points_soa.y_values[i] - expected_y).abs() < f64::EPSILON);
            assert!((points_soa.heading_values[i] - expected_heading).abs() < f64::EPSILON);
        }
    }

    #[test]
    fn verify_single_variable_correctness() {
        let mut points_aos = setup::create_aos_test_data(10);
        let mut points_soa = setup::create_soa_test_data(10);
        let aos_original = points_aos.clone();
        let soa_original = PointsSoA {
            x_values: points_soa.x_values.clone(),
            y_values: points_soa.y_values.clone(),
            heading_values: points_soa.heading_values.clone(),
        };
        single_variable::perform_aos(&mut points_aos);
        single_variable::perform_soa(&mut points_soa);
        for i in 0..10 {
            let expected_x = aos_original[i].x + 1.0;
            assert!((points_aos[i].x - expected_x).abs() < f64::EPSILON);
            assert!((points_soa.x_values[i] - expected_x).abs() < f64::EPSILON);
            // y and heading should be unchanged
            assert!((points_aos[i].y - aos_original[i].y).abs() < f64::EPSILON);
            assert!((points_aos[i].heading - aos_original[i].heading).abs() < f64::EPSILON);
            assert!((points_soa.y_values[i] - soa_original.y_values[i]).abs() < f64::EPSILON);
            assert!(
                (points_soa.heading_values[i] - soa_original.heading_values[i]).abs()
                    < f64::EPSILON
            );
        }
    }

    #[test]
    fn test_performance() {
        independent_operations::performance_test();
        dependent_operations::performance_test();
        single_variable::performance_test();
    }
}

fn main() {}
